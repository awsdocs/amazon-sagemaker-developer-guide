# Sharded Data Parallelism<a name="model-parallel-extended-features-pytorch-sharded-data-parallelism"></a>

*Sharded data parallelism* is a memory\-saving distributed training technique that splits the training state of a model \(model parameters, gradients, and optimizer states\) across GPUs in a data parallel group\. 

**Note**  
This feature is available in the SageMaker model parallel library v1\.11\.0 and later\.

When scaling up your training job to a large GPU cluster, you can reduce the per\-GPU memory footprint of the model by sharding the training state over multiple GPUs\. This returns two benefits: you can fit larger models, which would otherwise run out of memory with standard data parallelism, or you can increase the batch size using the freed\-up GPU memory\.

The standard data parallelism technique replicates the training states across the GPUs in the data parallel group, and performs gradient aggregation based on the `AllReduce` operation\. Sharded data parallelism modifies the standard data\-parallel distributed training procedure to account for the sharded nature of the optimizer states\. A group of ranks over which the model and optimizer states are sharded is called a *sharding group*\. The sharded data parallelism technique shards the trainable parameters of a model and corresponding gradients and optimizer states across the GPUs in the *sharding group*\.

SageMaker implements sharded data parallelism through the MiCS implementation, which is discussed in the AWS blog post [Near\-linear scaling of gigantic\-model training on AWS](https://www.amazon.science/blog/near-linear-scaling-of-gigantic-model-training-on-aws)\. In this implementation, you can set the sharding degree as a configurable parameter, which must be less than the data parallelism degree\. During each forward and backward pass, MiCS temporarily recombines the model parameters in all GPUs through the `AllGather` operation\. After the forward or backward pass of each layer, MiCS shards the parameters again to save GPU memory\. During the backward pass, MiCS reduces gradients and simultaneously shards them across GPUs through the `ReduceScatter` operation\. Finally, MiCS applies the local reduced and sharded gradients to their corresponding local parameter shards, using the local shards of optimizer states\. To bring down communication overhead, the SageMaker model parallel library prefetches the upcoming layers in forward or backward pass, and overlaps the network communication with the computation\.

The training state of the model is replicated across the sharding groups\. This means that before gradients are applied to the parameters, the `AllReduce` operation must take place across the sharding groups, in addition to the `ReduceScatter` operation that takes place within the sharding group\.

In effect, sharded data parallelism introduces a tradeoff between the communication overhead and GPU memory efficiency\. Using sharded data parallelism increases the communication cost, but the memory footprint per GPU \(excluding the memory usage due to activations\) is divided by the sharded data parallelism degree, thus larger models can be fit in the GPU cluster\.

The selected sharded data parallelism degree must evenly divide the data parallelism degree\. For example, for an 8\-way data parallelism job, choose 2, 4, or 8 for the sharded data parallelism degree\. While choosing the sharded data parallelism degree, we recommend that you start with a small number, and gradually increase it until the model fits in the memory together with the desired batch size\.

**Topics**
+ [How to Apply Sharded Data Parallelism to Your Training Job](#model-parallel-extended-features-pytorch-sharded-data-parallelism-how-to-use)
+ [Mixed Precision Training with Sharded Data Parallelism](#model-parallel-extended-features-pytorch-sharded-data-parallelism-16bits-training)
+ [Tips and Considerations for Using Sharded Data Parallelism](#model-parallel-extended-features-pytorch-sharded-data-parallelism-considerations)

## How to Apply Sharded Data Parallelism to Your Training Job<a name="model-parallel-extended-features-pytorch-sharded-data-parallelism-how-to-use"></a>

To use sharded data parallelism, apply the following modifications to your training script and estimator\.

**Adapt your PyTorch training script**

Follow the instructions at [Step 1: Modify a PyTorch Training Script](model-parallel-customize-training-script-pt.md) to wrap the model and optimizer objects with the `smdistributed.modelparallel.torch` wrappers of the `torch.nn.parallel` and `torch.distributed` modules\.

**Configure a SageMaker PyTorch estimator**

As part of configuring a SageMaker PyTorch estimator in [Step 2: Launch a Training Job Using the SageMaker Python SDK](model-parallel-sm-sdk.md), add the parameters for sharded data parallelism\. 

To turn on sharded data parallelism, add the `sharded_data_parallel_degree` parameter to the SageMaker PyTorch Estimator\. This parameter specifies the number of GPUs over which the training state is sharded\. The value for `sharded_data_parallel_degree` must be an integer between one and the data parallelism degree and must evenly divide the data parallelism degree\. Note that the library automatically detects the number of GPUs so thus the data parallel degree\. The following additional parameters are available for configuring sharded data parallelism\.
+ `"sdp_reduce_bucket_size"` *\(int, default: 5e8\)* – Specifies the size of [PyTorch DDP gradient buckets](https://pytorch.org/docs/stable/notes/ddp.html#internal-design) in number of elements of the default dtype\.
+ `"sdp_param_persistence_threshold"` *\(int, default: 1e6\)* – Specifies the size of a parameter tensor in number of elements that can persist at each GPU\. Sharded data parallelism splits each parameter tensor across GPUs of a data parallel group\. If the number of elements in the parameter tensor is smaller than this threshold, the parameter tensor is not split; this helps reduce communication overhead because the parameter tensor is replicated across data\-parallel GPUs\.
+ `"sdp_max_live_parameters"` *\(int, default: 1e9\)* – Specifies the maximum number of parameters that can simultaneously be in a recombined training state during the forward and backward pass\. Parameter fetching with the `AllGather` operation pauses when the number of active parameters reaches the given threshold\. Note that increasing this parameter increases the memory footprint\.
+ `"sdp_hierarchical_allgather"` *\(bool, default: True\)* – If set to `True`, the `AllGather` operation runs hierarchically: it runs within each node first, and then runs across nodes\. For multi\-node distributed training jobs, the hierarchical `AllGather` operation is automatically activated\.
+ `"sdp_gradient_clipping"` *\(float, default: 1\.0\)* – Specifies a threshold for gradient clipping the L2 norm of the gradients before propagating them backward through the model parameters\. When sharded data parallelism is activated, gradient clipping is also activated\. The default threshold is `1.0`\. Adjust this parameter if you have the exploding gradients problem\.

The following code shows an example of how to configure sharded data parallelism\.

```
import sagemaker
from sagemaker.pytorch import PyTorch

smp_options = {
    "enabled": True,
    "parameters": {
        # "pipeline_parallel_degree": 1,    # Optional, default is 1
        # "tensor_parallel_degree": 1,      # Optional, default is 1
        "ddp": True,
        # parameters for sharded data parallelism
        "sharded_data_parallel_degree": 2,              # Add this to activate sharded data parallelism
        "sdp_reduce_bucket_size": int(5e8),             # Optional
        "sdp_param_persistence_threshold": int(1e6),    # Optional
        "sdp_max_live_parameters": int(1e9),            # Optional
        "sdp_hierarchical_allgather": True,             # Optional
        "sdp_gradient_clipping": 1.0                    # Optional
    }
}

mpi_options = {
    "enabled" : True,                      # Required
    "processes_per_host" : 8               # Required
}

smp_estimator = PyTorch(
    entry_point="your_training_script.py", # Specify your train script
    role=sagemaker.get_execution_role(),
    instance_count=1,
    instance_type='ml.p3.16xlarge',
    framework_version='1.12.0',
    py_version='pyxy',
    distribution={
        "smdistributed": {"modelparallel": smp_options},
        "mpi": mpi_options
    },
    base_job_name="sharded-data-parallel-job"
)

smp_estimator.fit('s3://my_bucket/my_training_data/')
```

## Mixed Precision Training with Sharded Data Parallelism<a name="model-parallel-extended-features-pytorch-sharded-data-parallelism-16bits-training"></a>

To further save GPU memory with half\-precision floating point numbers and sharded data parallelism, you can activate 16\-bit floating point format \(FP16\) or [Brain floating point format](https://en.wikichip.org/wiki/brain_floating-point_format) \(BF16\) by adding one additional parameter to the distributed training configuration\.

**Note**  
You can't activate both data types in one training job, and the `fp16` and `bf16` parameters are mutually exclusive\.

**For FP16 Training with Sharded Data Parallelism**

To run FP16 training with sharded data parallelism, add `"fp16": True"` to the `smp_options` configuration dictionary\. In your training script, you can choose between the static and dynamic loss scaling options through the `smp.DistributedOptimizer` module\. For more information, see [FP16 Training with Model Parallelism](model-parallel-extended-features-pytorch-fp16.md)\.

```
smp_options = {
    "enabled": True,
    "parameters": {
        "ddp": True,
        "sharded_data_parallel_degree": 2,
        "fp16": True
    }
}
```

**For BF16 Training with Sharded Data Parallelism**

The sharded data parallelism feature of SageMaker supports training in BF16 data type\. The BF16 data type uses 8 bits to represent the exponent of a floating point number, while the FP16 data type uses 5 bits\. Preserving the 8 bits for the exponent allows to keep the same representation of the exponent of a 32\-bit single precision floating point \(FP32\) number\. This makes the conversion between FP32 and BF16 simpler and significantly less prone to cause overflow and underflow issues that arise often in FP16 training, especially when training larger models\. While both data types use 16 bits in total, this increased representation range for the exponent in the BF16 format comes at the expense of reduced precision\. For training large models, this reduced precision is often considered an acceptable trade\-off for the range and training stability\.

**Note**  
Currently, BF16 training works only when sharded data parallelism is activated\.

To run BF16 training with sharded data parallelism, add `"bf16": True` to the `smp_options` configuration dictionary\.

```
smp_options = {
    "enabled": True,
    "parameters": {
        "ddp": True,
        "sharded_data_parallel_degree": 2,
        "bf16": True
    }
}
```

## Tips and Considerations for Using Sharded Data Parallelism<a name="model-parallel-extended-features-pytorch-sharded-data-parallelism-considerations"></a>

Consider the following when using the SageMaker model parallel library's sharded data parallelism\.
+ Sharded data parallelism is compatible with FP16 training\. To run FP16 training, see [FP16 Training with Model Parallelism](model-parallel-extended-features-pytorch-fp16.md)\.
+ Sharded data parallelism currently is not compatible with [tensor parallelism](model-parallel-extended-features-pytorch-tensor-parallelism.md), [pipeline parallelism](model-parallel-intro.md#model-parallel-intro-pp), and [optimizer state sharding](model-parallel-extended-features-pytorch-optimizer-state-sharding.md)\. To activate sharded data parallelism, set tensor and pipeline parallelism degrees to 1, and turn off optimizer state sharding\. 

  The [activation checkpointing](model-parallel-extended-features-pytorch-activation-checkpointing.md) and [activation offloading](model-parallel-extended-features-pytorch-activation-offloading.md) features are compatible with sharded data parallelism\.
+ To use sharded data parallelism with gradient accumulation, set the `backward_passes_per_step` argument to the number of accumulation steps while wrapping your model with the [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.DistributedModel](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.DistributedModel) module\. This ensures that the gradient `AllReduce` operation across the model replication groups \(sharding groups\) takes place at the boundary of gradient accumulation\.
+ You can checkpoint your models trained with sharded data parallelism using the library's checkpointing APIs, `smp.save_checkpoint` and `smp.resume_from_checkpoint`\. For more information, see [Checkpointing Distributed Models and Optimizer States](model-parallel-extended-features-pytorch-checkpoint.md)\.
+ The behavior of the [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.delay_param_initialization](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.delay_param_initialization) configuration parameter changes under sharded data parallelism\. When these two features are simultaneously turned on, parameters are immediately initialized upon model creation in a sharded manner instead of delaying the parameter initialization, so that each rank initializes and stores its own shard of parameters\.
+ When sharded data parallelism is activated, the library performs gradient clipping internally when the `optimizer.step()` call runs\. You don't need to use utility APIs for gradient clipping, such as [torch\.nn\.utils\.clip\_grad\_norm\_\(\)](https://pytorch.org/docs/stable/generated/torch.nn.utils.clip_grad_norm_.html)\. To adjust the threshold value for gradient clipping, you can set it through the `sdp_gradient_clipping` parameter for the distribution parameter configuration when you construct the SageMaker PyTorch estimator, as shown in the [How to Apply Sharded Data Parallelism to Your Training Job](#model-parallel-extended-features-pytorch-sharded-data-parallelism-how-to-use) section\.