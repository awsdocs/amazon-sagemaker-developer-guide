# Extended Features of the SageMaker Model Parallel Library for PyTorch<a name="model-parallel-extended-features-pytorch"></a>

In addition to its [core features](https://docs.aws.amazon.com/sagemaker/latest/dg/model-parallel-core-features.html), the SageMaker distributed model parallel library offers memory\-saving features for training deep learning models with PyTorch: tensor parallelism, optimizer state sharding, activation checkpointing, and activation offloading\.

**Note**  
Extended memory\-saving features are available through Deep Learning Containers for PyTorch, which implements the SageMaker distributed model parallel library v1\.6\.0 or later\.

For each of the following features, you keep the same two\-step workflow shown in the [Run a SageMaker Distributed Training Job with Model Parallelism](model-parallel-use-api.md) section and add few additional parameters and code lines to the SageMaker `PyTorch` estimator and your training script\.

To find an example of how to use the extended features, see [Train GPT\-2 with PyTorch 1\.8\.1 and Tensor Parallelism Using the SageMaker Model Parallelism Library](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/model_parallel/gpt2/smp-train-gpt-simple.html)\.

**Topics**
+ [Tensor Parallelism](model-parallel-extended-features-pytorch-tensor-parallelism.md)
+ [Optimizer State Sharding](model-parallel-extended-features-pytorch-optimizer-state-sharding.md)
+ [Activation Checkpointing](model-parallel-extended-features-pytorch-activation-checkpointing.md)
+ [Activation Offloading](model-parallel-extended-features-pytorch-activation-offloading.md)
+ [Ranking Mechanism](model-parallel-extended-features-pytorch-ranking-mechanism.md)
+ [FP16 Training with Model Parallelism](model-parallel-extended-features-pytorch-fp16.md)
+ [Checkpointing Distributed Models and Optimizer States](model-parallel-extended-features-pytorch-checkpoint.md)
+ [Sharded Data Parallelism](model-parallel-extended-features-pytorch-sharded-data-parallelism.md)