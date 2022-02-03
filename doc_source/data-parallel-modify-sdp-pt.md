# Modify a PyTorch Training Script<a name="data-parallel-modify-sdp-pt"></a>

The following steps show you how to convert a PyTorch training script to utilize SageMaker's distributed data parallel library\.

The library APIs are designed to be a distributed backend for PyTorch Distributed Data Parallel \(DDP\) APIs\. For additional details on each data parallel API offered for PyTorch, see the [SageMaker distributed data parallel PyTorch API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html#api-documentation)\. Please also refer to PyTorch's documentation on [DDP](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html) and [distributed backends](https://pytorch.org/docs/stable/distributed.html)\.

Here you can find the PyTorch versions supported by SageMaker and the SageMaker distributed data parallel library.

| PyTorch version | SageMaker distributed data parallel library version | smdistributed-dataparallel integrated image URI |
| :---        |    :----:   |          ---: |
| v1.10.2     | smdistributed-dataparallel==v1.4.0 | 763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.10.2-gpu-py38-cu113-ubuntu20.04-sagemaker |
| v1.10.0     | smdistributed-dataparallel==v1.4.0 | 763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.10.0-gpu-py38-cu113-ubuntu20.04-sagemaker |
| v1.9.1      | smdistributed-dataparallel==v1.4.0 | 763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.9.1-gpu-py38-cu111-ubuntu20.04            |
| v1.8.1      | smdistributed-dataparallel==v1.3.0 | 763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.8.1-gpu-py36-cu111-ubuntu18.04            |

**Note**  
SageMaker's distributed data parallelism library supports Automatic Mixed Precision \(AMP\) out of the box\. No extra action is needed to enable AMP other than the framework\-level modifications to your training script\. If gradients are in FP16, the SageMaker data parallelism library runs its `AllReduce` operation in FP16\. For more information about implementing AMP APIs to your training script, see the following resources:  
[Frameworks \- PyTorch](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html#pytorch) in the *NVIDIA Deep Learning Performace documentation*
[Automatic Mixed Precision for Deep Learning](https://developer.nvidia.com/automatic-mixed-precision) in the *NVIDIA Developer Docs*
[Introducing native PyTorch automatic mixed precision for faster training on NVIDIA GPUs](https://pytorch.org/blog/accelerating-training-on-nvidia-gpus-with-pytorch-automatic-mixed-precision/) in the *PyTorch Blog*

1. Import the library’s PyTorch client, it will register with PyTorch as a distributed backend. Then import PyTorch's distributed module as usual. Specify backend as `smddp` when initializing process group\. 

   ```
   import torch
   
   import smdistributed.dataparallel.torch.torch_smddp
   
   import torch.distributed as dist
   
   from torch.nn.parallel import DistributedDataParallel as DDP
   
   dist.init_process_group(backend='smddp')
   ```

1. After parsing arguments and defining a batch size parameter \(for example, `batch_size=args.batch_size`\), add a 2\-line of code to resize the batch size per worker \(GPU\)\. PyTorch's DataLoader operation does not automatically handle the batch resizing for distributed training\.

   ```
   batch_size //= dist.get_world_size()
   batch_size = max(batch_size, 1)
   ```

1. Pin each GPU to a single SageMaker data parallel library process with `local_rank`—this refers to the relative rank of the process within a given node\.

   You can retreive local rank information from the environment variable `LOCAL_RANK`.

   ```
   import os
   import torch
   local_rank = os.environ["LOCAL_RANK"]
   torch.cuda.set_device(local_rank)
   ```

1. Wrap the PyTorch model with PyTorch’s DDP\. 

   ```
   model = ...
   # Wrap model with PyTorch's DistributedDataParallel
   model = DDP(model)
   ```

1. Modify the`torch.utils.data.distributed.DistributedSampler` to include the cluster’s information\. Set `num_replicas` to the total number of processes(GPUs) participating in training across all the nodes in the cluster\. This is called `world_size`\. You can get `world_size` with the `torch.distributed.get_world_size()` API\. This is invoked in the following code as `dist.get_world_size()`\. Also supply the process's rank among all processes using `torch.distributed.get_rank()`\. This is invoked as `dist.get_rank()`\. 

   ```
   train_sampler = DistributedSampler(train_dataset, num_replicas=dist.get_world_size(), rank=dist.get_rank())
   ```

1. Modify your script to save checkpoints only on the leader process(rank 0)\. The leader process has a synchronized model\. This also avoids other processes overwriting the checkpoints and possibly corrupting the checkpoints\. 

The following is an example PyTorch training script for distributed training with the library: 

```
import os
import torch
# SageMaker data parallel: Import the library PyTorch API
import smdistributed.dataparallel.torch.torch_smddp

# SageMaker data parallel: Import PyTorch's distributed API
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel as DDP

# SageMaker data parallel: Initialize the process group
dist.init_process_group(backend='smddp')

class Net(nn.Module):
    ...
    # Define model

def train(...):
    ...
    # Model training

def test(...):
    ...
    # Model evaluation

def main():
    
    # SageMaker data parallel: Scale batch size by world size
    batch_size //= dist.get_world_size()
    batch_size = max(batch_size, 1)

    # Prepare dataset
    train_dataset = torchvision.datasets.MNIST(...)
 
    # SageMaker data parallel: Set num_replicas and rank in DistributedSampler
    train_sampler = torch.utils.data.distributed.DistributedSampler(
            train_dataset,
            num_replicas=dist.get_world_size(),
            rank=dist.get_rank())
 
    train_loader = torch.utils.data.DataLoader(..)
 
    # SageMaker data parallel: Wrap the PyTorch model with the library's DDP
    model = DDP(Net().to(device))
    
    # SageMaker data parallel: Pin each GPU to a single library process.
    local_rank = os.environ["LOCAL_RANK"] 
    torch.cuda.set_device(local_rank)
    model.cuda(local_rank)
    
    # Train
    optimizer = optim.Adadelta(...)
    scheduler = StepLR(...)
    for epoch in range(1, args.epochs + 1):
        train(...)
        if rank == 0:
            test(...)
        scheduler.step()

    # SageMaker data parallel: Save model on master node.
    if dist.get_rank() == 0:
        torch.save(...)

if __name__ == '__main__':
    main()
```

For more advanced usage, see the [SageMaker distributed data parallel PyTorch API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html#api-documentation)\. 

After you have completed adapting your training script, move on to the next topic: [Run a SageMaker Distributed Data Parallel Training Job](data-parallel-use-api.md) using the SageMaker Python SDK\. 