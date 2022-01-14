# Activation Offloading<a name="model-parallel-extended-features-pytorch-activation-offloading"></a>

When activation checkpointing and pipeline parallelism are turned on and the number of microbatches is greater than one, *activation offloading* is an additional feature that can further reduce memory usage\. Activation offloading asynchronously moves the checkpointed activations corresponding to their microbatches that are not currently running in the CPU\. Right before the GPU needs the activations for the microbatch’s backward pass, this functionality prefetches the offloaded activations back from the CPU\.

## How to Use Activation Offloading<a name="model-parallel-extended-for-pytorch-activation-offloading"></a>

Use activation offloading to reduce memory usage when **the number of microbatches is greater than 1, and activation checkpointing is enabled** \(see [Activation Checkpointing](model-parallel-extended-features-pytorch-activation-checkpointing.md)\)\. When the activation checkpointing is not used, activation offloading has no effect\. When it is used with only one microbatch, it does not save memory\.

To use activation offloading, set `"offload_activations": True` in the `modelparallel` configuration\.

Activation offloading moves the checkpointed activations in `nn.Sequential` modules to CPU asynchronously\. The data transfer over the PCIe link overlaps with GPU computation\. The offloading happens immediately, as soon as the forward pass for a particular checkpointed layer is computed\. The activations are loaded back to the GPU shortly before they are needed for the backward pass of a particular microbatch\. The CPU\-GPU transfer similarly overlaps with computation\. 

To adjust how early the activations are loaded back into the GPU, you can use the configuration parameter `"activation_loading_horizon"` \(default is set to 4, must be `int` larger than 0\)\. A larger activation loading horizon would cause the activations to be loaded back to the GPU earlier\. If the horizon is too large, the memory\-saving impact of activation offloading might be diminished\. If the horizon is too small, the activations may not be loaded back in time, reducing the amount of overlap and degrading performance\.

**Tip**  
Activation offloading can be useful for large models with over a hundred billion parameters\.

**Configure a SageMaker PyTorch estimator**

```
mpi_options = {
    "enabled" : True,
    "processes_per_host" : 8,               # 8 processes
    "custom_mpi_options" : "--mca btl_vader_single_copy_mechanism none "
}

smp_options = {
    "enabled":True,
    "parameters": {
        "microbatches": 4,
        "pipeline_parallel_degree": 2,    # alias for "partitions"
        "placement_strategy": "cluster",
        "tensor_parallel_degree": 2,      # tp over 2 devices
        "ddp": True,
        "offload_activations": True,
        "activation_loading_horizon": 4   # optional. default is 4.
    }
}
```