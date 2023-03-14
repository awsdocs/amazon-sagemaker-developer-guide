# Sharded Data Parallelism<a name="model-parallel-extended-features-pytorch-sharded-data-parallelism"></a>

*Sharded data parallelism* is a memory\-saving distributed training technique that splits the training state of a model \(model parameters, gradients, and optimizer states\) across GPUs in a data parallel group\. 

**Note**  
Sharded data parallelism is available in the SageMaker model parallelism library v1\.11\.0 and later\.

When scaling up your training job to a large GPU cluster, you can reduce the per\-GPU memory footprint of the model by sharding the training state over multiple GPUs\. This returns two benefits: you can fit larger models, which would otherwise run out of memory with standard data parallelism, or you can increase the batch size using the freed\-up GPU memory\.

The standard data parallelism technique replicates the training states across the GPUs in the data parallel group, and performs gradient aggregation based on the `AllReduce` operation\. Sharded data parallelism modifies the standard data\-parallel distributed training procedure to account for the sharded nature of the optimizer states\. A group of ranks over which the model and optimizer states are sharded is called a *sharding group*\. The sharded data parallelism technique shards the trainable parameters of a model and corresponding gradients and optimizer states across the GPUs in the *sharding group*\.

SageMaker implements sharded data parallelism through the MiCS implementation, which is discussed in the AWS blog post [Near\-linear scaling of gigantic\-model training on AWS](https://www.amazon.science/blog/near-linear-scaling-of-gigantic-model-training-on-aws)\. In this implementation, you can set the sharding degree as a configurable parameter, which must be less than the data parallelism degree\. During each forward and backward pass, MiCS temporarily recombines the model parameters in all GPUs through the `AllGather` operation\. After the forward or backward pass of each layer, MiCS shards the parameters again to save GPU memory\. During the backward pass, MiCS reduces gradients and simultaneously shards them across GPUs through the `ReduceScatter` operation\. Finally, MiCS applies the local reduced and sharded gradients to their corresponding local parameter shards, using the local shards of optimizer states\. To bring down communication overhead, the SageMaker model parallelism library prefetches the upcoming layers in the forward or backward pass, and overlaps the network communication with the computation\.

The training state of the model is replicated across the sharding groups\. This means that before gradients are applied to the parameters, the `AllReduce` operation must take place across the sharding groups, in addition to the `ReduceScatter` operation that takes place within the sharding group\.

In effect, sharded data parallelism introduces a tradeoff between the communication overhead and GPU memory efficiency\. Using sharded data parallelism increases the communication cost, but the memory footprint per GPU \(excluding the memory usage due to activations\) is divided by the sharded data parallelism degree, thus larger models can be fit in the GPU cluster\.

The selected sharded data parallelism degree must evenly divide the data parallelism degree\. For example, for an 8\-way data parallelism job, choose 2, 4, or 8 for the sharded data parallelism degree\. While choosing the sharded data parallelism degree, we recommend that you start with a small number, and gradually increase it until the model fits in the memory together with the desired batch size\.

**Topics**
+ [How to Apply Sharded Data Parallelism to Your Training Job](#model-parallel-extended-features-pytorch-sharded-data-parallelism-how-to-use)
+ [Sharded data parallelism with SMDDP Collectives](#model-parallel-extended-features-pytorch-sharded-data-parallelism-smddp-collectives)
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

## Sharded data parallelism with SMDDP Collectives<a name="model-parallel-extended-features-pytorch-sharded-data-parallelism-smddp-collectives"></a>

The SageMaker data parallelism library offers collective communication primitives \(SMDDP collectives\) optimized for the AWS infrastructure\. It achieves optimization by adopting an all\-to\-all\-type communication pattern by making use of [Elastic Fabric Adapter \(EFA\)](https://aws.amazon.com/hpc/efa/), resulting in high\-throughput and less latency\-sensitive collectives, offloading the communication\-related processing to the CPU, and freeing up GPU cycles for computation\. On large clusters, SMDDP Collectives can offer improvements in distributed training performance by up to 40% compared to NCCL\. For case studies and benchmark results, see the blog [New performance improvements in the Amazon SageMaker model parallelism library](http://aws.amazon.com/blogs/machine-learning/new-performance-improvements-in-amazon-sagemaker-model-parallel-library/)\.

**Note**  
Sharded data parallelism with SMDDP Collectives is available in the SageMaker model parallelism library v1\.13\.0 and later, and the SageMaker data parallelism library v1\.6\.0 and later\. See also [Supported configurations](#sharded-data-parallelism-smddp-collectives-supported-config) to use sharded data parallelism with SMDDP Collectives\.

In sharded data parallelism, which is a commonly used technique in large\-scale distributed training, the `AllGather` collective is used to reconstitute the sharded layer parameters for forward and backward pass computations, in parallel with GPU computation\. For large models, performing the `AllGather` operation efficiently is critical to avoid GPU bottleneck problems and slowing down training speed\. When sharded data parallelism is activated, SMDDP Collectives drops into these performance\-critical `AllGather` collectives, improving training throughput\.

**Train with SMDDP Collectives**

When your training job has sharded data parallelism activated and meets the [Supported configurations](#sharded-data-parallelism-smddp-collectives-supported-config), SMDDP Collectives are automatically activated\. Internally, SMDDP Collectives optimize the `AllGather` collective to be performant on the AWS infrastructure and falls back to NCCL for all other collectives\. Furthermore, under unsupported configurations, all collectives, including `AllGather`, automatically use the NCCL backend\.

Since the SageMaker model parallelism library version 1\.13\.0, the `"ddp_dist_backend"` parameter is added to the `modelparallel` options\. The default value for this configuration parameter is `"auto"`, which uses SMDDP Collectives whenever possible, and falls back to NCCL otherwise\. To force the library to always use NCCL, specify `"nccl"` to the `"ddp_dist_backend"` configuration parameter\. 

The following code example shows how to set up a PyTorch estimator using the sharded data parallelism with the `"ddp_dist_backend"` parameter, which is set to `"auto"` by default and, therefore, optional to add\. 

```
import sagemaker
from sagemaker.pytorch import PyTorch

smp_options = {
    "enabled":True,
    "parameters": {                        
        "partitions": 1,
        "ddp": True,
        "sharded_data_parallel_degree": 64
        "bf16": True,
        "ddp_dist_backend": "auto"  # Specify "nccl" to force to use NCCL.
    }
}

mpi_options = {
    "enabled" : True,                      # Required
    "processes_per_host" : 8               # Required
}

smd_mp_estimator = PyTorch(
    entry_point="your_training_script.py", # Specify your train script
    source_dir="location_to_your_script",
    role=sagemaker.get_execution_role(),
    instance_count=8,
    instance_type='ml.p4d.24xlarge',
    framework_version='1.12.1',
    py_version='py3',
    distribution={
        "smdistributed": {"modelparallel": smp_options},
        "mpi": mpi_options
    },
    base_job_name="sharded-data-parallel-demo",
)

smd_mp_estimator.fit('s3://my_bucket/my_training_data/')
```

**Supported configurations**

The `AllGather` operation with SMDDP Collectives are activated in training jobs when all the following configuration requirements are met\.
+ The sharded data parallelism degree greater than 1
+ `Instance_count` greater than 1 
+ `Instance_type` equal to `ml.p4d.24xlarge` 
+ SageMaker training container for PyTorch v1\.12\.1 or later
+ The SageMaker data parallelism library v1\.6\.0 or later
+ The SageMaker model parallelism library v1\.13\.0 or later

**Performance and memory tuning**

SMDDP Collectives utilize additional GPU memory\. There are two environment variables to configure the GPU memory usage depending on different model training use cases\.
+ `SMDDP_AG_SCRATCH_BUFFER_SIZE_BYTES` – During the SMDDP `AllGather` operation, the `AllGather` input buffer is copied into a temporary buffer for inter\-node communication\. The `SMDDP_AG_SCRATCH_BUFFER_SIZE_BYTES` variable controls the size \(in bytes\) of this temporary buffer\. If the size of the temporary buffer is smaller than the `AllGather` input buffer size, the `AllGather` collective falls back to use NCCL\.
  + Default value: 16 \* 1024 \* 1024 \(16 MB\)
  + Acceptable values: any multiple of 8192
+  `SMDDP_AG_SORT_BUFFER_SIZE_BYTES` – The `SMDDP_AG_SORT_BUFFER_SIZE_BYTES` variable is to size the temporary buffer \(in bytes\) to hold data gathered from inter\-node communication\. If the size of this temporary buffer is smaller than `1/8 * sharded_data_parallel_degree * AllGather input size`, the `AllGather` collective falls back to use NCCL\.
  + Default value: 128 \* 1024 \* 1024 \(128 MB\)
  + Acceptable values: any multiple of 8192

**Tuning guidance on the buffer size variables**

The default values for the environment variables should work well for most use cases\. We recommend tuning these variables only if training runs into the out\-of\-memory \(OOM\) error\. 

The following list discusses some tuning tips to reduce the GPU memory footprint of SMDDP Collectives while retaining the performance gain from them\.
+ Tuning `SMDDP_AG_SCRATCH_BUFFER_SIZE_BYTES`
  + The `AllGather` input buffer size is smaller for smaller models\. Hence, the required size for `SMDDP_AG_SCRATCH_BUFFER_SIZE_BYTES` can be smaller for models with fewer parameters\.
  + The `AllGather` input buffer size decreases as `sharded_data_parallel_degree` increases, because the model gets sharded across more GPUs\. Hence, the required size for `SMDDP_AG_SCRATCH_BUFFER_SIZE_BYTES` can be smaller for training jobs with large values for `sharded_data_parallel_degree`\.
+ Tuning `SMDDP_AG_SORT_BUFFER_SIZE_BYTES`
  + The amount of data gathered from inter\-node communication is less for models with fewer parameters\. Hence, the required size for `SMDDP_AG_SORT_BUFFER_SIZE_BYTES` can be smaller for such models with fewer number of parameters\.

Some collectives might fall back to use NCCL; hence, you might not get the performance gain from the optimized SMDDP collectives\. If additional GPU memory is available for use, you can consider increasing the values of `SMDDP_AG_SCRATCH_BUFFER_SIZE_BYTES` and `SMDDP_AG_SORT_BUFFER_SIZE_BYTES` to benefit from the performance gain\.

The following code shows how you can configure the environment variables by appending them to `mpi_options` in the distribution parameter for the PyTorch estimator\.

```
import sagemaker
from sagemaker.pytorch import PyTorch

smp_options = {
    .... # All modelparallel configuration options go here
}

mpi_options = {
    "enabled" : True,                      # Required
    "processes_per_host" : 8               # Required
}

# Use the following two lines to tune values of the environment variables for buffer
mpioptions += "-x SMDDP_AG_SCRATCH_BUFFER_SIZE_BYTES=8192" 
mpioptions += "-x SMDDP_AG_SORT_BUFFER_SIZE_BYTES=8192"

smd_mp_estimator = PyTorch(
    entry_point="your_training_script.py", # Specify your train script
    source_dir="location_to_your_script",
    role=sagemaker.get_execution_role(),
    instance_count=8,
    instance_type='ml.p4d.24xlarge',
    framework_version='1.12.1',
    py_version='py3',
    distribution={
        "smdistributed": {"modelparallel": smp_options},
        "mpi": mpi_options
    },
    base_job_name="sharded-data-parallel-demo-with-tuning",
)

smd_mp_estimator.fit('s3://my_bucket/my_training_data/')
```

## Mixed Precision Training with Sharded Data Parallelism<a name="model-parallel-extended-features-pytorch-sharded-data-parallelism-16bits-training"></a>

To further save GPU memory with half\-precision floating point numbers and sharded data parallelism, you can activate 16\-bit floating point format \(FP16\) or [Brain floating point format](https://en.wikichip.org/wiki/brain_floating-point_format) \(BF16\) by adding one additional parameter to the distributed training configuration\.

**Note**  
Mixed precision training with sharded data parallelism is available in the SageMaker model parallelism library v1\.11\.0 and later\.

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

Consider the following when using the SageMaker model parallelism library's sharded data parallelism\.
+ Sharded data parallelism is compatible with FP16 training\. To run FP16 training, see [FP16 Training with Model Parallelism](model-parallel-extended-features-pytorch-fp16.md)\.
+ Sharded data parallelism currently is not compatible with [tensor parallelism](model-parallel-extended-features-pytorch-tensor-parallelism.md), [pipeline parallelism](model-parallel-intro.md#model-parallel-intro-pp), and [optimizer state sharding](model-parallel-extended-features-pytorch-optimizer-state-sharding.md)\. To activate sharded data parallelism, set tensor and pipeline parallelism degrees to 1, and turn off optimizer state sharding\. 

  The [activation checkpointing](model-parallel-extended-features-pytorch-activation-checkpointing.md) and [activation offloading](model-parallel-extended-features-pytorch-activation-offloading.md) features are compatible with sharded data parallelism\.
+ To use sharded data parallelism with gradient accumulation, set the `backward_passes_per_step` argument to the number of accumulation steps while wrapping your model with the [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.DistributedModel](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.DistributedModel) module\. This ensures that the gradient `AllReduce` operation across the model replication groups \(sharding groups\) takes place at the boundary of gradient accumulation\.
+ You can checkpoint your models trained with sharded data parallelism using the library's checkpointing APIs, `smp.save_checkpoint` and `smp.resume_from_checkpoint`\. For more information, see [Checkpointing Distributed Models and Optimizer States](model-parallel-extended-features-pytorch-checkpoint.md)\.
+ The behavior of the [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.delay_param_initialization](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.delay_param_initialization) configuration parameter changes under sharded data parallelism\. When these two features are simultaneously turned on, parameters are immediately initialized upon model creation in a sharded manner instead of delaying the parameter initialization, so that each rank initializes and stores its own shard of parameters\.
+ When sharded data parallelism is activated, the library performs gradient clipping internally when the `optimizer.step()` call runs\. You don't need to use utility APIs for gradient clipping, such as [torch\.nn\.utils\.clip\_grad\_norm\_\(\)](https://pytorch.org/docs/stable/generated/torch.nn.utils.clip_grad_norm_.html)\. To adjust the threshold value for gradient clipping, you can set it through the `sdp_gradient_clipping` parameter for the distribution parameter configuration when you construct the SageMaker PyTorch estimator, as shown in the [How to Apply Sharded Data Parallelism to Your Training Job](#model-parallel-extended-features-pytorch-sharded-data-parallelism-how-to-use) section\.