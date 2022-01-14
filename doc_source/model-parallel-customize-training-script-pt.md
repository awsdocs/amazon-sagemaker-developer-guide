# Modify a PyTorch Training Script<a name="model-parallel-customize-training-script-pt"></a>

In this section, you learn how to modify PyTorch training scripts to configure the SageMaker distributed model parallel library for auto\-partitioning and manual partitioning\.


**PyTorch versions supported by SageMaker and the SageMaker distributed model parallel library**  

| PyTorch version | SageMaker distributed model parallel library version | `smdistributed-modelparallel` integrated image URI | 
| --- | --- | --- | 
| v1\.10\.0 |  smdistributed\-modelparallel==v1\.5\.0  |  `763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.10.0-gpu-py38-cu113-ubuntu20.04-sagemaker`  | 
| v1\.9\.1 |  smdistributed\-modelparallel==v1\.4\.0  |  `763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.9.1-gpu-py38-cu111-ubuntu20.04`  | 
| v1\.8\.1\* |  smdistributed\-modelparallel==v1\.6\.0  | 763104351884\.dkr\.ecr\.<region>\.amazonaws\.com/pytorch\-training:1\.8\.1\-gpu\-py36\-cu111\-ubuntu18\.04  | 

\* Deep Learning Container for PyTorch 1\.8\.1 is updated with the SageMaker distributed model parallel library v1\.6\.0 that provides extended features for PyTorch\. For more information, see [Extended Features of the SageMaker Model Parallel Library for PyTorch](model-parallel-extended-features-pytorch.md)\.

The following topics show examples of training scripts that you can use to configure SageMaker's model parallel library for auto\-partitioning and manual partitioning PyTorch models\. We recommend that you review the [Important Considerations](#model-parallel-pt-considerations) and [Unsupported Framework Features](#model-parallel-pt-unsupported-features) before creating a training script\.

The required modifications you must make to your training script to use the library are listed in [PyTorch](#model-parallel-customize-training-script-pt-16)\.

If you want to use manual partitioning, also review [Manual Partitioning with PyTorch](#model-parallel-customize-training-script-pt-16-hvd)\. 

**Tip**  
For end\-to\-end, runnable notebook examples that demonstrate how to use a PyTorch training script with the SageMaker distributed model parallel library, see [PyTorch Examples](distributed-training-notebook-examples.md#distributed-training-notebook-examples-pytorch)\.

Note that auto\-partitioning is enabled by default\. Unless otherwise specified, the following scripts use auto\-partitioning\. 

**Topics**
+ [Important Considerations](#model-parallel-pt-considerations)
+ [Unsupported Framework Features](#model-parallel-pt-unsupported-features)
+ [PyTorch](#model-parallel-customize-training-script-pt-16)
+ [Manual Partitioning with PyTorch](#model-parallel-customize-training-script-pt-16-hvd)

## Important Considerations<a name="model-parallel-pt-considerations"></a>

When you configure a PyTorch training script using SageMaker's distributed model parallel library, you should be aware of the following:
+ If you are using an optimization technique that relies on global gradient norms, for example gradient norm from the entire model, such as some variants of LAMB optimizer or global gradient clipping, you need to gather all the norms across the model partitions for correctness\. You can use the library’s communication basic data types to do this\.
+ All `torch.Tensor` arguments to the forward methods of the `nn.Modules` in your model must be used in the computation of the module output\. In other words, the library does not support the case where there is a `torch.Tensor` argument to a module on which the module output does not depend\.
+ The argument to the `smp.DistributedModel.backward()` call must depend on all model outputs\. In other words, there cannot be an output from the `smp.DistributedModel.forward` call that is not used in the computation of the tensor that is fed into the `smp.DistributedModel.backward` call\.
+ If there are `torch.cuda.synchronize()` calls in your code, you might need to call `torch.cuda.set_device(smp.local_rank())` immediately before the synchronize call\. Otherwise unnecessary CUDA contexts might be created in device 0, which will needlessly consume memory\.
+ Since the library places `nn.Modules` on different devices, the modules in the model must not depend on any global state that is modified inside `smp.step`\. Any state that remains fixed throughout training, or that is modified outside `smp.step` in a way that is visible to all processes, is allowed\.
+ You don’t need to move the model to GPU \(for example, using `model.to(device)`\) when using the library\. If you try to move the model to GPU before the model is partitioned \(before the first `smp.step` call\), the move call is ignored\. The library automatically moves the part of the model assigned to a rank to its GPU\. Once training with the library starts, don’t move the model to CPU and use it, as it won’t have correct parameters for modules not assigned to the partition held by the process\. If you want to retrain a model or use it for inference without the library after it was trained using the model parallel library, the recommended way is to save the full model using our checkpointing API and load it back to a regular PyTorch Module\.
+ If you have a list of modules such that output of one feeds into another, replacing that list with `nn.Sequential` can significantly improve performance\.
+ The weight update \(`optimizer.step()`\) needs to happen outside of `smp.step` because that is when the entire backward pass is done and gradients are ready\. When using a hybrid model with model and data parallelism, at this point, Allreduce of gradients is also guaranteed to finish\.
+ When using the library in combination with data parallelism, make sure that the number of batches on all data parallel ranks is the same so that Allreduce does not hang waiting for a rank which is not participating in the step\.
+ If you launch a training job using an ml\.p4d instance type \(such as ml\.p4d\.24xlarge\), you must set the data loader variable `num_workers=0`\. For example, you may define your `DataLoader` as follows:

  ```
  dataloader = torch.utils.data.DataLoader(
              data,
              batch_size=batch_size,
              num_workers=0,
              pin_memory=True,
              drop_last=True,
              shuffle=shuffle,
          )
  ```
+ The inputs to `smp.step` must be the model inputs generated by `DataLoader`\. This is because `smp.step` internally splits the input tensors along the batch dimension and pipelines them\. This means that passing `DataLoader` itself to the `smp.step` function to generate the model inputs inside does not work\. 

  For example, if you define a `DataLoader` as follows:

  ```
  train_loader = torch.utils.data.DataLoader(dataset, batch_size=64, drop_last=True)
  ```

  You should access the model inputs generated by `train_loader` and pass those to an `smp.step` decorated function\. Do not pass `train_loader` directly to the `smp.step` function\.

  ```
  def train(model, device, train_loader, optimizer):
      model.train()
      for batch_idx, (data, target) in enumerate(train_loader):
          ...
          _, loss_mb = train_step(model, data, target)
          ...
  
  @smp.step
  def train_step(model, data, target):
      ...
      return output, loss
  ```
+ The input tensors to `smp.step` must be moved to the current device using `.to()` API, which must take place after the `torch.cuda.set_device(local_rank())` call\.

  For example, you may define the `train` function as follows\. This function adds `data` and `target` to the current device using `.to()` API before using those input tensors to call `train_step`\.

  ```
  def train(model, device, train_loader, optimizer):
      model.train()
      for batch_idx, (data, target) in enumerate(train_loader):
          # smdistributed: Move input tensors to the GPU ID used by the current process,
          # based on the set_device call.
          data, target = data.to(device), target.to(device)
          optimizer.zero_grad()
          # Return value, loss_mb is a StepOutput object
          _, loss_mb = train_step(model, data, target)
  
          # smdistributed: Average the loss across microbatches.
          loss = loss_mb.reduce_mean()
  
          optimizer.step()
  ```

  The input tensors to this `smp.set` decorated function have been moved to the current device in the `train` function above\. The model does *not* need to be moved to the current device\. The library automatically moves the part of the model assigned to a rank to its GPU\.

  ```
  @smp.step
  def train_step(model, data, target):
      output = model(data)
      loss = F.nll_loss(output, target, reduction="mean")
      model.backward(loss)
      return output, loss
  ```

## Unsupported Framework Features<a name="model-parallel-pt-unsupported-features"></a>

The following PyTorch features are unsupported by SageMaker's distributed model parallel library:
+ If you use data parallelism with the native [PyTorch DDP](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html), the [https://pytorch.org/docs/stable/generated/torch.nn.parallel.DistributedDataParallel.html](https://pytorch.org/docs/stable/generated/torch.nn.parallel.DistributedDataParallel.html) wrapper module is not supported by the library\. The library internally manages integrating with PyTorch DDP, including parameter broadcast and gradient AllReduce\. When using the library, module buffers are only broadcast once at the start of training\. If your model has module buffers that need to be synchronized across data parallel groups at each step, you can do so through the `torch.distributed` API, using the process group that can be obtained via `smp.get_dp_process_group()`\.
+ For mixed precision training, the `apex.amp` module is not supported\. The recommended way to use the library with automatic mixed\-precision is to use `torch.cuda.amp`, with the exception of using `smp.amp.GradScaler` instead of the implementation in torch\.
+ `torch.jit.ScriptModules` or `ScriptFunctions` are not supported by `smp.DistributedModel`\.
+ `apex` : `FusedLayerNorm`, `FusedAdam`, `FusedLAMB`, and `FusedNovoGrad` from `apex` are not supported\. You can use the library implementations of these through `smp.optimizers` and `smp.nn` APIs instead\.

## PyTorch<a name="model-parallel-customize-training-script-pt-16"></a>

The following training script changes are required to run a PyTorch model with SageMaker's distributed model parallel library:

1. Import and initialize the library with [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#smp.init](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#smp.init)\.

1. Wrap the model with [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_pytorch.html#smp.DistributedModel](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_pytorch.html#smp.DistributedModel)\. Be mindful that any tensors returned from the `forward` method of the underlying `nn.Module` object will be broadcast across model\-parallel devices, incurring communication overhead, so any tensors that are not needed outside the call method \(such as intermediate activations\) should not be returned\.

1. Wrap the optimizer with [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_pytorch.html#smp.DistributedOptimizer](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_pytorch.html#smp.DistributedOptimizer)\.

1. Use the returned `DistributedModel` object instead of a user model\.

1. Put the forward and backward logic in a step function and decorate it with [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#smp.init](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#smp.init)\.

1. Restrict each process to its own device through `torch.cuda.set_device(smp.local_rank())`\.

1. Move the input tensors to the GPU using the `.to()` API before the `smp.step` call \(see example below\)\.

1. Replace `torch.Tensor.backward` and `torch.autograd.backward` with `DistributedModel.backward`\.

1. Perform post\-processing on the outputs across microbatches using [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#StepOutput](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#StepOutput) methods such as `reduce_mean`\.

1. If there is an evaluation step, similarly place the forward logic inside an `smp.step`\-decorated function and post\-process the outputs using [`StepOutput` API](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#StepOutput)\.

1. Set `drop_last=True` in `DataLoader`\. Alternatively, manually skip a batch in the training loop if the batch size is not divisible by the number of microbatches\.

To learn more about the SageMaker's distributed model parallel library API, refer to the [API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\. 

```
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchnet.dataset import SplitDataset
from torchvision import datasets

import smdistributed.modelparallel.torch as smp

class GroupedNet(nn.Module):
    def __init__(self):
        super(GroupedNet, self).__init__()
        # define layers

    def forward(self, x):
        # define forward pass and return model outputs


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
        # smdistributed: Move input tensors to the GPU ID used by the current process,
        # based on the set_device call.
        data, target = data.to(device), target.to(device)
        optimizer.zero_grad()
        # Return value, loss_mb is a StepOutput object
        _, loss_mb = train_step(model, data, target)

        # smdistributed: Average the loss across microbatches.
        loss = loss_mb.reduce_mean()

        optimizer.step()

# smdistributed: initialize the backend
smp.init()

# smdistributed: Set the device to the GPU ID used by the current process.
# Input tensors should be transferred to this device.
torch.cuda.set_device(smp.local_rank())
device = torch.device("cuda")

# smdistributed: Download only on a single process per instance.
# When this is not present, the file is corrupted by multiple processes trying
# to download and extract at the same time
dataset = datasets.MNIST("../data", train=True, download=False)

# smdistributed: Shard the dataset based on data-parallel ranks
if smp.dp_size() > 1:
    partitions_dict = {f"{i}": 1 / smp.dp_size() for i in range(smp.dp_size())}
    dataset = SplitDataset(dataset, partitions=partitions_dict)
    dataset.select(f"{smp.dp_rank()}")

# smdistributed: Set drop_last=True to ensure that batch size is always divisible
# by the number of microbatches
train_loader = torch.utils.data.DataLoader(dataset, batch_size=64, drop_last=True)

model = GroupedNet()
optimizer = optim.Adadelta(model.parameters(), lr=4.0)

# smdistributed: Use the DistributedModel container to provide the model
# to be partitioned across different ranks. For the rest of the script,
# the returned DistributedModel object should be used in place of
# the model provided for DistributedModel class instantiation.
model = smp.DistributedModel(model)
optimizer = smp.DistributedOptimizer(optimizer)

train(model, device, train_loader, optimizer)
```

## Manual Partitioning with PyTorch<a name="model-parallel-customize-training-script-pt-16-hvd"></a>

Use [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_pytorch.html#smp.DistributedOptimizer](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_pytorch.html#smp.DistributedOptimizer) context managers to place modules in specific devices\. Any module not placed in any `smp.partition` contexts is placed in the `default_partition`\. The `default_partition` needs to be provided if `auto_partition` is set to `False`\. The modules that are created within a specific `smp.partition` context are placed on the corresponding partition\.

To learn more about the SageMaker's distributed model parallel library API, refer to the [API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\. 

```
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchnet.dataset import SplitDataset
from torchvision import datasets

import smdistributed.modelparallel.torch as smp

class GroupedNet(nn.Module):
    def __init__(self):
        super(GroupedNet, self).__init__()
        with smp.partition(0):
            # define child modules on device 0
        with smp.partition(1):
            # define child modules on device 1

    def forward(self, x):
        # define forward pass and return model outputs


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
        # smdistributed: Move input tensors to the GPU ID used by the current process,
        # based on the set_device call.
        data, target = data.to(device), target.to(device)
        optimizer.zero_grad()
        # Return value, loss_mb is a StepOutput object
        _, loss_mb = train_step(model, data, target)

        # smdistributed: Average the loss across microbatches.
        loss = loss_mb.reduce_mean()

        optimizer.step()

# smdistributed: initialize the backend
smp.init()

# smdistributed: Set the device to the GPU ID used by the current process.
# Input tensors should be transferred to this device.
torch.cuda.set_device(smp.local_rank())
device = torch.device("cuda")

# smdistributed: Download only on a single process per instance.
# When this is not present, the file is corrupted by multiple processes trying
# to download and extract at the same time
dataset = datasets.MNIST("../data", train=True, download=False)

# smdistributed: Shard the dataset based on data-parallel ranks
if smp.dp_size() > 1:
    partitions_dict = {f"{i}": 1 / smp.dp_size() for i in range(smp.dp_size())}
    dataset = SplitDataset(dataset, partitions=partitions_dict)
    dataset.select(f"{smp.dp_rank()}")

# smdistributed: Set drop_last=True to ensure that batch size is always divisible
# by the number of microbatches
train_loader = torch.utils.data.DataLoader(dataset, batch_size=64, drop_last=True)

model = GroupedNet()
optimizer = optim.Adadelta(model.parameters(), lr=4.0)

# smdistributed: Use the DistributedModel container to provide the model
# to be partitioned across different ranks. For the rest of the script,
# the returned DistributedModel object should be used in place of
# the model provided for DistributedModel class instantiation.
model = smp.DistributedModel(model)
optimizer = smp.DistributedOptimizer(optimizer)

train(model, device, train_loader, optimizer)
```