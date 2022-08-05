# Run a SageMaker Distributed Model Parallel Training Job with Tensor Parallelism<a name="model-parallel-extended-features-pytorch-tensor-parallelism-examples"></a>

In this section, you learn:
+ How to configure a SageMaker PyTorch estimator and the SageMaker distributed model parallelism option to use tensor parallelism\.
+ How to adapt your training script using the extended `smdistributed.modelparallel` modules for tensor parallelism\.

To learn more about the `smdistributed.modelparallel` modules, see the [SageMaker distributed model parallel APIs](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html) in the *SageMaker Python SDK documentation*\.

**Topics**
+ [Tensor parallelism alone](#model-parallel-extended-features-pytorch-tensor-parallelism-alone)
+ [Tensor parallelism combined with pipeline parallelism](#model-parallel-extended-features-pytorch-tensor-and-pipeline-parallelism)

## Tensor parallelism alone<a name="model-parallel-extended-features-pytorch-tensor-parallelism-alone"></a>

The following is an example of a distributed training option to activate tensor parallelism alone, without pipeline parallelism\. Configure the `mpi_options` and `smp_options` dictionaries to specify distributed training options to the SageMaker `PyTorch` estimator\.

**Note**  
Extended memory\-saving features are available through Deep Learning Containers for PyTorch, which implements the SageMaker distributed model parallel library v1\.6\.0 or later\.

**Configure a SageMaker PyTorch estimator**

```
mpi_options = {
    "enabled" : True,
    "processes_per_host" : 8,               # 8 processes
    "custom_mpi_options" : "--mca btl_vader_single_copy_mechanism none "
}
               
smp_options = {
    "enabled":True,
    "parameters": {
        "pipeline_parallel_degree": 1,    # alias for "partitions"
        "placement_strategy": "cluster",
        "tensor_parallel_degree": 4,      # tp over 4 devices
        "ddp": True
    }
}
              
smd_mp_estimator = PyTorch(
    entry_point='your_training_script.py', # Specify
    role=role,
    instance_type='ml.p3.16xlarge',
    sagemaker_session=sagemaker_session,
    framework_version='1.12.0',
    py_version='py36',
    instance_count=1,
    distribution={
        "smdistributed": {"modelparallel": smp_options},
        "mpi": mpi_options
    },
    base_job_name="SMD-MP-demo",
)

smd_mp_estimator.fit('s3://my_bucket/my_training_data/')
```

**Tip**  
To find a complete list of parameters for `distribution`, see [Configuration Parameters for Model Parallelism](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html) in the SageMaker Python SDK documentation\.

**Adapt your PyTorch training script**

The following example training script shows how to adapt the SageMaker distributed model parallelism library to a training script\. In this example, it is assumed that the script is named `your_training_script.py`\. 

```
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchnet.dataset import SplitDataset
from torchvision import datasets

import smdistributed.modelparallel.torch as smp

class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(1, 32, 3, 1)
        self.conv2 = nn.Conv2d(32, 64, 3, 1)
        self.fc1 = nn.Linear(9216, 128)
        self.fc2 = nn.Linear(128, 10)

    def forward(self, x):
        x = self.conv1(x)
        x = F.relu(x)
        x = self.conv2(x)
        x = F.relu(x)
        x = F.max_pool2d(x, 2)
        x = torch.flatten(x, 1)
        x = self.fc1(x)
        x = F.relu(x)
        x = self.fc2(x)
        return F.log_softmax(x, 1)

def train(model, device, train_loader, optimizer):
    model.train()
    for batch_idx, (data, target) in enumerate(train_loader):
        # smdistributed: Move input tensors to the GPU ID used by
        # the current process, based on the set_device call.
        data, target = data.to(device), target.to(device)
        optimizer.zero_grad()
        output = model(data)
        loss = F.nll_loss(output, target, reduction="mean")
        loss.backward()
        optimizer.step()

# smdistributed: Initialize the backend
smp.init()

# smdistributed: Set the device to the GPU ID used by the current process.
# Input tensors should be transferred to this device.
torch.cuda.set_device(smp.local_rank())
device = torch.device("cuda")

# smdistributed: Download only on a single process per instance.
# When this is not present, the file is corrupted by multiple processes trying
# to download and extract at the same time
if smp.local_rank() == 0:
    dataset = datasets.MNIST("../data", train=True, download=False)
smp.barrier()

# smdistributed: Shard the dataset based on data parallel ranks
if smp.dp_size() > 1:
    partitions_dict = {f"{i}": 1 / smp.dp_size() for i in range(smp.dp_size())}
    dataset = SplitDataset(dataset, partitions=partitions_dict)
    dataset.select(f"{smp.dp_rank()}")

train_loader = torch.utils.data.DataLoader(dataset, batch_size=64)

# smdistributed: Enable tensor parallelism for all supported modules in the model
# i.e., nn.Linear in this case. Alternatively, we can use
# smp.set_tensor_parallelism(model.fc1, True)
# to enable it only for model.fc1
with smp.tensor_parallelism():
    model = Net()

# smdistributed: Use the DistributedModel wrapper to distribute the
# modules for which tensor parallelism is enabled
model = smp.DistributedModel(model)

optimizer = optim.AdaDelta(model.parameters(), lr=4.0)
optimizer = smp.DistributedOptimizer(optimizer)

train(model, device, train_loader, optimizer)
```

## Tensor parallelism combined with pipeline parallelism<a name="model-parallel-extended-features-pytorch-tensor-and-pipeline-parallelism"></a>

The following is an example of a distributed training option that enables tensor parallelism combined with pipeline parallelism\. Set up the `mpi_options` and `smp_options` parameters to specify distributed model parallel options with tensor parallelism when you configure a SageMaker `PyTorch` estimator\.

**Note**  
Extended memory\-saving features are available through Deep Learning Containers for PyTorch, which implements the SageMaker distributed model parallel library v1\.6\.0 or later\.

**Configure a SageMaker PyTorch estimator**

```
mpi_options = {
    "enabled" : True,
    "processes_per_host" : 8,               # 8 processes
    "custom_mpi_options" : "--mca btl_vader_single_copy_mechanism none "
}
               
smp_options = {
    "enabled":True,
    "parameters": {
    "microbatches": 4,
        "pipeline_parallel_degree": 2,    # alias for "partitions"
        "placement_strategy": "cluster",
        "tensor_parallel_degree": 2,      # tp over 2 devices
        "ddp": True
    }
}
              
smd_mp_estimator = PyTorch(
    entry_point='your_training_script.py', # Specify
    role=role,
    instance_type='ml.p3.16xlarge',
    sagemaker_session=sagemaker_session,
    framework_version='1.12.0',
    py_version='py36',
    instance_count=1,
    distribution={
        "smdistributed": {"modelparallel": smp_options},
        "mpi": mpi_options
    },
    base_job_name="SMD-MP-demo",
)

smd_mp_estimator.fit('s3://my_bucket/my_training_data/')  
```

<a name="model-parallel-extended-features-pytorch-tensor-and-pipeline-parallelism-script"></a>**Adapt your PyTorch training script**

The following example training script shows how to adapt the SageMaker distributed model parallelism library to a training script\. Note that the training script now includes the `smp.step` decorator: 

```
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchnet.dataset import SplitDataset
from torchvision import datasets

import smdistributed.modelparallel.torch as smp

class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(1, 32, 3, 1)
        self.conv2 = nn.Conv2d(32, 64, 3, 1)
        self.fc1 = nn.Linear(9216, 128)
        self.fc2 = nn.Linear(128, 10)

    def forward(self, x):
        x = self.conv1(x)
        x = F.relu(x)
        x = self.conv2(x)
        x = F.relu(x)
        x = F.max_pool2d(x, 2)
        x = torch.flatten(x, 1)
        x = self.fc1(x)
        x = F.relu(x)
        x = self.fc2(x)
        return F.log_softmax(x, 1)


# smdistributed: Define smp.step. Return any tensors needed outside.
@smp.step
def train_step(model, data, target):
    output = model(data)
    loss = F.nll_loss(output, target, reduction="mean")
    model.backward(loss)
    return output, loss

def train(model, device, train_loader, optimizer):
    model.train()
    for batch_idx, (data, target) in enumerate(train_loader):
        # smdistributed: Move input tensors to the GPU ID used by
        # the current process, based on the set_device call.
        data, target = data.to(device), target.to(device)
        optimizer.zero_grad()
        # Return value, loss_mb is a StepOutput object
        _, loss_mb = train_step(model, data, target)

        # smdistributed: Average the loss across microbatches.
        loss = loss_mb.reduce_mean()

        optimizer.step()

# smdistributed: Initialize the backend
smp.init()

# smdistributed: Set the device to the GPU ID used by the current process.
# Input tensors should be transferred to this device.
torch.cuda.set_device(smp.local_rank())
device = torch.device("cuda")

# smdistributed: Download only on a single process per instance.
# When this is not present, the file is corrupted by multiple processes trying
# to download and extract at the same time
if smp.local_rank() == 0:
    dataset = datasets.MNIST("../data", train=True, download=False)
smp.barrier()

# smdistributed: Shard the dataset based on data parallel ranks
if smp.dp_size() > 1:
    partitions_dict = {f"{i}": 1 / smp.dp_size() for i in range(smp.dp_size())}
    dataset = SplitDataset(dataset, partitions=partitions_dict)
    dataset.select(f"{smp.dp_rank()}")

# smdistributed: Set drop_last=True to ensure that batch size is always divisible
# by the number of microbatches
train_loader = torch.utils.data.DataLoader(dataset, batch_size=64, drop_last=True)

model = Net()

# smdistributed: enable tensor parallelism only for model.fc1
smp.set_tensor_parallelism(model.fc1, True)

# smdistributed: Use the DistributedModel container to provide the model
# to be partitioned across different ranks. For the rest of the script,
# the returned DistributedModel object should be used in place of
# the model provided for DistributedModel class instantiation.
model = smp.DistributedModel(model)

optimizer = optim.AdaDelta(model.parameters(), lr=4.0)
optimizer = smp.DistributedOptimizer(optimizer)

train(model, device, train_loader, optimizer)
```