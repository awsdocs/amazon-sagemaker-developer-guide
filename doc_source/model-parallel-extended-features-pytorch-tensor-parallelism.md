# Tensor Parallelism<a name="model-parallel-extended-features-pytorch-tensor-parallelism"></a>

*Tensor parallelism* is a type of model parallelism in which specific model weights, gradients, and optimizer states are split across devices\. In contrast to pipeline parallelism, which keeps individual weights intact but partitions the *set* of weights, tensor parallelism splits individual weights\. This typically involves distributed computation of specific operations, modules, or layers of the model\.

Tensor parallelism is required in cases in which a single parameter consumes most of the GPU memory \(such as large embedding tables with a large vocabulary size or a large softmax layer with a large number of classes\)\. In this case, treating this large tensor or operation as an atomic unit is inefficient and impedes balance of the memory load\. 

Tensor parallelism is also useful for extremely large models in which a pure pipelining is simply not enough\. For example, with GPT\-3\-scale models that require partitioning over tens of instances, a pure microbatch pipelining is inefficient because the pipeline depth becomes too high and the overhead becomes prohibitively large\.

**Topics**
+ [How Tensor Parallelism Works](model-parallel-extended-features-pytorch-tensor-parallelism-how-it-works.md)
+ [Run a SageMaker Distributed Model Parallel Training Job with Tensor Parallelism](model-parallel-extended-features-pytorch-tensor-parallelism-examples.md)
+ [Instructions for Checkpointing with Tensor Parallelism](model-parallel-extended-features-pytorch-saving-loading-checkpoints.md)
+ [Support for Hugging Face Transformer Models](model-parallel-extended-features-pytorch-hugging-face.md)