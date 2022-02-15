# SageMaker Distributed Model Parallel Configuration Tips and Pitfalls<a name="model-parallel-customize-tips-pitfalls"></a>

Review the following tips and pitfalls before using Amazon SageMaker's distributed model parallel library\. This list includes tips that are applicable across frameworks\. For TensorFlow and PyTorch specific tips, see [Modify a TensorFlow Training Script](model-parallel-customize-training-script-tf.md) and [Modify a PyTorch Training Script](model-parallel-customize-training-script-pt.md), respectively\. 

## Batch Size and Number of Microbatches<a name="model-parallel-customize-tips-pitfalls-batch-size"></a>
+ The library is most efficient when the batch size is increased\. For use cases where the model fits within a single device, but can only be trained with a small batch size, batch size can and should be increased after the library is integrated\. Model parallelism saves memory for large models, enabling you to train using batch sizes that previously did not fit in memory\.
+ Choosing a number of microbatches that is too small or too large can lower performance\. The library executes each microbatch sequentially in each device, so microbatch size \(batch size divided by number of microbatches\) must be large enough to fully utilize each GPU\. At the same time, pipeline efficiency increases with the number of microbatches, so striking the right balance is important\. Typically, a good starting point is to try 2 or 4 microbatches, increasing the batch size to the memory limit, and then experiment with larger batch sizes and numbers of microbatches\. As the number of microbatches is increased, larger batch sizes might become feasible if an interleaved pipeline is used\.
+ Your batch size must be always divisible by the number of microbatches\. Note that depending on the size of the dataset, sometimes the last batch of every epoch can be of a smaller size than the rest, and this smaller batch needs to be divisible by the number of microbatches as well\. If it is not, you can set `drop_remainder=True` in the `tf.Dataset.batch()` call \(in TensorFlow\), or set `drop_last=True` in `DataLoader` \(in PyTorch\), so that this last, small batch is not used\. If you are using a different API for the data pipeline, you might need to manually skip the last batch whenever it is not divisible by the number of microbatches\.

## Manual Partitioning<a name="model-parallel-customize-tips-pitfalls-manual-partitioning"></a>
+ If you use manual partitioning, be mindful of the parameters that are consumed by multiple operations and modules in your model, such as the embedding table in transformer architectures\. Modules that share the same parameter must be placed in the same device for correctness\. When auto\-partitioning is used, the library automatically enforces this constraint\.

## Data Preparation<a name="model-parallel-customize-tips-pitfalls-data-preparation"></a>
+ If the model takes multiple inputs, make sure you seed the random operations in your data pipeline \(e\.g\., shuffling\) with `smp.dp_rank()`\. If the dataset is being deterministically sharded across data parallel devices, make sure that the shard is indexed by `smp.dp_rank()`\. This is to make sure that the order of the data seen on all ranks that form a model partition is consistent\.

## Returning Tensors from `smp.DistributedModel`<a name="model-parallel-customize-tips-pitfalls-return-tensors"></a>
+ Any tensor that is returned from the `smp.DistributedModel.call` \(for TensorFlow\) or `smp.DistributedModel.forward` \(for PyTorch\) function is broadcast to all other ranks, from the rank that computed that particular tensor\. As a result, any tensor that is not needed outside the call and forward methods \(intermediate activations, for example\) should not be returned, as this causes needless communication and memory overhead and hurts performance\.

## The `@smp.step` Decorator<a name="model-parallel-customize-tips-pitfalls-smp-step-decorator"></a>
+ If an `smp.step`\-decorated function has a tensor argument that does not have a batch dimension, the argument name must be provided in the `non_split_inputs` list when calling `smp.step`\. This prevents the library from attempting to split the tensor into microbatches\. For more information see [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_common_api.html](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_common_api.html) in the API documentation\.

## Delaying Parameter Initialization<a name="model-parallel-customize-tips-pitfalls-delaying-param-initialization"></a>

For very large models over 100 billion parameters, weight initialization through the CPU memory might result in an out\-of\-memory error\. To get around this, the library offers `smp.delay_param_initialization` context manager\. This delays the physical allocation of parameters until they move to GPU during the first execution of a `smp.step`\-decorated function\. This avoids unnecessary memory usage of the CPU during the initialization of training\. Use the context manager when you create a model object as shown in the following code\.

```
with smp.delay_param_initialization(enabled=True):    
    model = MyModel()
```

## Tensor Parallelism for PyTorch<a name="model-parallel-customize-tips-pitfalls-tensor-parallelism-pytorch"></a>
+ If you are using a seed for deterministic results, set the seed based on `smp.dp_rank()` \(for example, `torch.manual_seed(42 + smp.dp_rank())`\)\. If you do not do this, different partitions of an `nn.Parameter` are initialized in the same way, impacting convergence\. 
+ SageMaker’s model parallelism library uses NCCL to implement collectives needed for the distribution of the modules\. Especially for smaller models, if too many NCCL calls are scheduled on the GPU at the same time, memory usage might increase because of additional space used by NCCL\. To counteract this, `smp` throttles the NCCL calls so that the number of ongoing NCCL operations at any given time is less than or equal to a given limit\. The default limit is 8, but this can be adjusted using the environment variable `SMP_NCCL_THROTTLE_LIMIT`\. If you observe more memory usage than you expect while using tensor parallelism, you can try reducing this limit\. However, choosing a limit that is too small might cause throughput loss\. To disable throttling altogether, you can set `SMP_NCCL_THROTTLE_LIMIT=-1`\. 
+ The following identity, which holds when the degree of tensor parallelism is 1, does not hold when the degree of tensor parallelism is greater than 1: `smp.mp_size() * smp.dp_size() == smp.size()`\. This is because the tensor parallel group is part of both the model parallelism group and the data parallelism group\. If your code has existing references to `mp_rank`, `mp_size`, `MP_GROUP`, and so on, and if you want to work with only the pipeline parallel group, you might need to replace the references with `smp.pp_size()`\. The following identities are always true: 
  +  `smp.mp_size() * smp.rdp_size() == smp.size()` 
  +  `smp.pp_size() * smp.dp_size() == smp.size()` 
  +  `smp.pp_size() * smp.tp_size() * smp.rdp_size() == smp.size()` 
+ Since the `smp.DistributedModel` wrapper modifies the model parameters when tensor parallelism is enabled, the optimizer should be created after calling `smp.DistributedModel`, with the distributed parameters\. For example, the following does not work: 

  ```
  ## WRONG
  model = MyModel()
  optimizer = SomeOptimizer(model.parameters())
  model = smp.DistributedModel(model)  # optimizer now has outdated parameters! 
  ```

  Instead, the optimizer should be created with the parameters of the `smp.DistributedModel` as follows:

  ```
  ## CORRECT
  model = smp.DistributedModel(MyModel())
  optimizer = SomeOptimizer(model.optimizers())
  ```
+ When a module is replaced with its distributed counterpart through tensor parallelism, the distributed module does not inherit its weights from the original module, and initializes new weights\. This means that, for instance, if the weights need to be initialized in a particular call \(for example, through a `load_state_dict` call\), this needs to happen after the `smp.DistributedModel` call, once the module distribution takes place\. 
+ When accessing the parameters of distributed modules directly, note that the weight does not have the same shape as the original module\. For instance,  

  ```
  with smp.tensor_parallelism():
      linear = nn.Linear(60, 60)
  
  # will pass
  assert tuple(linear.weight.shape) == (60, 60)
  
  distributed_linear = smp.DistributedModel(linear)
  
  # will fail. the number of input channels will have been divided by smp.tp_size()
  assert tuple(distributed_linear.module.weight.shape) == (60, 60)
  ```
+ Using `torch.utils.data.distributed.DistributedSampler` is strongly recommended for tensor parallelism\. This ensures that every data parallel rank receives the same number of data samples, which prevents hangs that might result from different `dp_rank`s taking a different number of steps\. 
+ If you use the `join` API of PyTorch's `DistributedDataParallel` class to handle cases in which different data parallel ranks have different numbers of batches, you still need to make sure that ranks that are in the same `TP_GROUP` have the same number of batches; otherwise the communication collectives used in distributed execution of modules may hang\. Ranks that are in different `TP_GROUP`s can have different numbers of batches, as long as `join` API is used\. 
+ If you want to checkpoint your model and use tensor parallelism, consider the following: 
  + To avoid stalling and race conditions while saving and loading models when you use tensor paralleism, make sure you call appropriate functions from the following model and optimizer states inside a reduced\-data parallelism rank\.
  + If you are transitioning an existing pipeline parallel script and enabling tensor parallel for the script, ensure that you modify any `if smp.dp_rank() == 0` block used for saving and loading with `if smp.rdp_rank() == 0` blocks\. Otherwise, it might cause your training job to stall\. 
  + Full checkpointing of optimizer states is currently not supported with tensor parallelism\. If you want to resume training from a checkpoint, use partial checkpointing instead\. 

  For more information about checkpointing a model with tensor parallelism, see [Instructions for Checkpointing with Tensor Parallelism](model-parallel-extended-features-pytorch-saving-loading-checkpoints.md)\.