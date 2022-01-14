# Activation Checkpointing<a name="model-parallel-extended-features-pytorch-activation-checkpointing"></a>

*Activation checkpointing* \(or *gradient checkpointing*\) is a technique to reduce memory usage by clearing activations of certain layers and recomputing them during a backward pass\. Effectively, this trades extra computation time for reduced memory usage\. If a module is checkpointed, at the end of a forward pass, the inputs to and outputs from the module stay in memory\. Any intermediate tensors that would have been part of the computation inside that module are freed up during the forward pass\. During the backward pass of checkpointed modules, these tensors are recomputed\. At this point, the layers beyond this checkpointed module have finished their backward pass, so the peak memory usage with checkpointing can be lower\.

## How to Use Activation Checkpointing<a name="model-parallel-extended-for-pytorch-activation-checkpointing-how-to-use"></a>

With `smdistributed.modelparallel`, you can use activation checkpointing at the granularity of a module\. For all `torch.nn` modules except `torch.nn.Sequential`, you can only checkpoint a module tree if it lies within one partition from the perspective of pipeline parallelism\. In case of the `torch.nn.Sequential` module, each module tree inside the sequential module needs to completely lie within one partition for activation checkpointing to work\. When you use manual partitioning, you need to be aware of these restrictions\.

When you use [automated model partitioning](https://docs.aws.amazon.com/sagemaker/latest/dg/model-parallel-core-features.html#model-parallel-automated-model-splitting), you can find the partitioning assignment logs starting with `Partition assignments:` in the training job logs\. If a module is partitioned across multiple ranks \(for example, with one descendant on one rank and another descendant on a different rank\), the library ignores the attempt to checkpoint the module and raises a warning message that the module won't be checkpointed\.

**Note**  
The SageMaker model parallel library supports both overlapping and non\-overlapping `allreduce` in combination with checkpointing\. 

**Note**  
PyTorch’s native checkpointing API is not compatible with `smdistributed.modelparallel`\.

**Example 1:** The following sample code shows how to use activation checkpointing when you have a model definition in your script\.

```
import torch.nn as nn
import torch.nn.functional as F

from smdistributed.modelparallel.torch.patches.checkpoint import checkpoint

class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(1, 32, 3, 1)
        self.conv2 = nn.Conv2d(32, 64, 3, 1)
        self.fc1 = nn.Linear(9216, 128)
        self.fc2 = nn.Linear(128, 10)

    def forward(self, x):
        x = self.conv1(x)
        x = self.conv2(x)
        x = F.max_pool2d(x, 2)
        x = torch.flatten(x, 1)
        # This call of fc1 will be checkpointed
        x = checkpoint(self.fc1, x)
        x = self.fc2(x)
        return F.log_softmax(x, 1)
```

**Example 2:** The following sample code shows how to use activation checkpointing when you have a sequential model in your script\.

```
import torch.nn as nn
from smdistributed.modelparallel.torch.patches.checkpoint import checkpoint_sequential

class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.seq = nn.Sequential(
            nn.Conv2d(1,20,5),
            nn.ReLU(),
            nn.Conv2d(20,64,5),
            nn.ReLU()
        )

    def forward(self, x):
        # This call of self.seq will be checkpointed
        x = checkpoint_sequential(self.seq, x)
        return F.log_softmax(x, 1)
```

**Example 3:** The following sample code shows how to use activation checkpointing when you import a prebuilt model from a library, such as PyTorch and Hugging Face Transformers\. Whether you checkpoint sequential modules or not, you need to do the following: 

1. Wrap the model by `smp.DistributedModel()`\.

1. Define an object for sequancial layers\.

1. Wrap the sequantial layer object by `smp.set_activation_checkpointig()`\.

```
import smdistributed.modelparallel.torch as smp
from transformers import AutoModelForCausalLM

smp.init()
model = AutoModelForCausalLM(*args, **kwargs)
model = smp.DistributedModel(model)

# Call set_activation_checkpointing API
transformer_layers = model.module.module.module.transformer.seq_layers
smp.set_activation_checkpointing(
    transformer_layers, pack_args_as_tuple=True, strategy='each')
```