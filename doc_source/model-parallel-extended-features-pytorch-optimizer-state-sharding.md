# Optimizer State Sharding<a name="model-parallel-extended-features-pytorch-optimizer-state-sharding"></a>

*Optimizer state sharding* is a useful memory\-saving technique that shards the optimizer state \(the set of weights that describes the state of optimizer\) across data parallel device groups\. You can use optimizer state sharding whenever you use a stateful optimizer \(such as Adam\) or an FP16 optimizer \(which stores both FP16 and FP32 copies of the parameters\)\.

## How to Use Optimizer State Sharding<a name="model-parallel-extended-features-pytorch-optimizer-state-sharding-how-to-use"></a>

You can turn on *optimizer state sharding* by setting `"shard_optimizer_state": True` in the `modelparallel` configuration\. 

When this feature is turned on, the library partitions the set of model parameters based on the data parallelism degree\. The gradients corresponding to the `i`th partition get reduced only at the `i`th data parallel rank\. At the end of the first call to an `smp.step` decorator function, the optimizer wrapped by `smp.DistributedOptimizer` redefines its parameters to be only limited to those parameters corresponding to the partition of the current data parallel rank\. The redefined parameters are called *virtual parameters* and share underlying storage with the original parameters\. During the first call to `optimizer.step`, the optimizer states are created based on these redefined parameters, which are sharded because of the original partition\. After the optimizer update, the AllGather operation \(as part of the `optimizer.step` call\) runs across the data parallel ranks to achieve consistent parameter states\.

**Tip**  
Optimizer state sharding can be useful when the degree of data parallelism is greater than 1 and the model has more than a billion parameters\.   
The degree of data parallelism is calculated by `(processes_per_host * instance_count / pipeline_parallel_degree)`, and the `smp.dp_size()` function handles the sizing in the background\.

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
        "shard_optimizer_state": True
    }
}
```

**Adapt your PyTorch training script**

See [Adapt your PyTorch training script](model-parallel-extended-features-pytorch-tensor-parallelism-examples.md#model-parallel-extended-features-pytorch-tensor-and-pipeline-parallelism-script) in the *Tensor parallelism combined with pipeline parallelism* section\. There’s no additional modification required for the script\.