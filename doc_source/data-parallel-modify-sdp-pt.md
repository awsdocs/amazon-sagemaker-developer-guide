# Modify a PyTorch Training Script<a name="data-parallel-modify-sdp-pt"></a>

In the SageMaker data parallel library v1\.4\.0 and later, the library is available as a backend option for the [PyTorch distributed package](https://pytorch.org/tutorials/beginner/dist_overview.html)\. You only need to import the library once at the top of your training script and set it as the PyTorch distributed backend during initialization\. With the single line of backend specification, you can keep your PyTorch training script unchanged and directly use the PyTorch distributed modules\. To find the latest API documentation for the library, see the [SageMaker distributed data parallel APIs for PyTorch](https://sagemaker.readthedocs.io/en/stable/api/training/sdp_versions/latest/smd_data_parallel_pytorch.html) in the *SageMaker Python SDK documentation*\. To learn more about the PyTorch distributed package and backend options, see [Distributed communication package \- torch\.distributed](https://pytorch.org/docs/stable/distributed.html)\.


**PyTorch versions supported by SageMaker and the SageMaker distributed data parallel library**  

| PyTorch version | SageMaker distributed data parallel library version | `smdistributed-dataparallel` integrated image URI | URL to the binary files of the library | 
| --- | --- | --- | --- | 
| v1\.10\.2 |  smdistributed\-dataparallel==v1\.4\.0  |  763104351884\.dkr\.ecr\.<region>\.amazonaws\.com/pytorch\-training:1\.10\.2\-gpu\-py38\-cu113\-ubuntu20\.04\-sagemaker  | https://smdataparallel\.s3\.amazonaws\.com/binary/pytorch/1\.10\.2/cu113/2022\-02\-18/smdistributed\_dataparallel\-1\.4\.0\-cp38\-cp38\-linux\_x86\_64\.whl | 
| v1\.8\.1 | smdistributed\-dataparallel==v1\.2\.3  | 763104351884\.dkr\.ecr\.<region>\.amazonaws\.com/pytorch\-training:1\.8\.1\-gpu\-py36\-cu111\-ubuntu18\.04 | https://smdataparallel\.s3\.amazonaws\.com/binary/pytorch/1\.8\.1/cu111/2021\-12\-13/smdistributed\_dataparallel\-1\.2\.3\-cp36\-cp36m\-linux\_x86\_64\.whl | 

**Important**  
Because the SageMaker distributed data parallelism library v1\.4\.0 and later works as a backend of PyTorch distributed, the following [smdistributed APIs](https://sagemaker.readthedocs.io/en/stable/api/training/sdp_versions/latest/smd_data_parallel_pytorch.html#pytorch-api) for the PyTorch distributed package are deprecated\.  
`smdistributed.dataparallel.torch.distributed` is deprecated\. Use the [torch\.distributed](https://pytorch.org/docs/stable/distributed.html) package instead\.
`smdistributed.dataparallel.torch.parallel.DistributedDataParallel` is deprecated\. Use the [torch\.nn\.parallel\.DistributedDataParallel](https://pytorch.org/docs/stable/generated/torch.nn.parallel.DistributedDataParallel.html) API instead\.
If you need to use the previous versions of the library \(v1\.3\.0 or before\), see the [archived SageMaker distributed data parallel library documentation](https://sagemaker.readthedocs.io/en/stable/api/training/sdp_versions/latest.html#documentation-archive) in the *SageMaker Python SDK documentation*\.

## Use the SageMaker Distributed Data Parallel Library as the Backend of `torch.distributed`<a name="data-parallel-enable-smddp-backend"></a>

To use the SageMaker distributed data parallel library, the only thing you need to do is to import the SageMaker distributed data parallel library’s PyTorch client \(`smdistributed.dataparallel.torch.torch_smddp`\)\. The client registers `smddp` as a backend for PyTorch\. When you initialize the PyTorch distributed process group using the `torch.distributed.init_process_group` API, make sure you specify `'smddp'` to the `backend` argument\.

```
import smdistributed.dataparallel.torch.torch_smddp
import torch.distributed as dist

dist.init_process_group(backend='smddp')
```

**Note**  
The `smddp` backend currently does not support creating subprocess groups with the `torch.distributed.new_group()` API\. You cannot use the `smddp` backend concurrently with other process group backends such as NCCL and Gloo\.

If you already have a working PyTorch script and only need to add the backend specification, you can proceed to [Using the SageMaker PyTorch Estimator](data-parallel-use-api.md#data-parallel-pytorch-api) in the [Step 2: Launch a SageMaker Distributed Training Job Using the SageMaker Python SDK](data-parallel-use-api.md) topic\. 

If you still need to modify your training script to properly use the PyTorch distributed package, follow the rest of the procedures on this page\.

## Preparing a PyTorch Training Script for Distributed Training<a name="data-parallel-how-to-modify-sdp-pt"></a>

The following steps provide additional tips on how to prepare your training script to successfully run a distributed training job using PyTorch\.

**Note**  
In v1\.4\.0, the SageMaker distributed data parallel library supports the following collective primitive data types of the [torch\.distributed](https://pytorch.org/docs/stable/distributed.html) interface: `all_reduce`, `broadcast`, `reduce`, `all_gather`, and `barrier`\.

1. Import the PyTorch distributed modules\.

   ```
   import torch
   import torch.distributed as dist
   from torch.nn.parallel import DistributedDataParallel as DDP
   ```

1. After parsing arguments and defining a batch size parameter \(for example, `batch_size=args.batch_size`\), add two lines of code to resize the batch size per worker \(GPU\)\. PyTorch's DataLoader operation does not automatically handle the batch resizing for distributed training\.

   ```
   batch_size //= dist.get_world_size()
   batch_size = max(batch_size, 1)
   ```

1. Pin each GPU to a single SageMaker data parallel library process with `local_rank`—this refers to the relative rank of the process within a given node\.

   You can retrieve the rank of the process from the `LOCAL_RANK` environment variable\.

   ```
   import os
   local_rank = os.environ["LOCAL_RANK"]
   torch.cuda.set_device(local_rank)
   ```

1. After defining a model, wrap it with the PyTorch `DistributedDataParallel` API\.

   ```
   model = ...
   
   # Wrap the model with the PyTorch DistributedDataParallel API
   model = DDP(model)
   ```

1. When you call the `torch.utils.data.distributed.DistributedSampler` API, specify the total number of processes \(GPUs\) participating in training across all the nodes in the cluster\. This is called `world_size`, and you can retrieve the number from the `torch.distributed.get_world_size()` API\. Also, specify the rank of each process among all processes using the `torch.distributed.get_rank()` API\.

   ```
   from torch.utils.data.distributed import DistributedSampler
   
   train_sampler = DistributedSampler(
     train_dataset, 
     num_replicas = dist.get_world_size(), 
     rank = dist.get_rank()
   )
   ```

1. Modify your script to save checkpoints only on the leader process \(rank 0\)\. The leader process has a synchronized model\. This also avoids other processes overwriting the checkpoints and possibly corrupting the checkpoints\.

   ```
   if dist.get_rank() == 0:
     torch.save(...)
   ```

The following example code shows the structure of a PyTorch training script with `smddp` as the backend\. 

```
import os
import torch

# SageMaker data parallel: Import the library PyTorch API
import smdistributed.dataparallel.torch.torch_smddp

# SageMaker data parallel: Import PyTorch's distributed API
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel as DDP

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
    train_dataset = torchvision.datasets.MNIST(...)
 
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

After you have completed adapting your training script, proceed to [Step 2: Launch a SageMaker Distributed Training Job Using the SageMaker Python SDK](data-parallel-use-api.md)\. 