# Step 1: Modify Your Own Training Script Using SageMaker's Distributed Model Parallel Library<a name="model-parallel-customize-training-script"></a>

Use this section to learn how to customize your training script to use the core features of the Amazon SageMaker model parallelism library\. To use the library\-specific API functions and parameters, we recommend you use this documentation alongside the [SageMaker model parallel library APIs](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html) in the *SageMaker Python SDK documentation*\.

The training script examples provided in these sections are simplified and designed to highlight the required changes you must make to use the library\. For end\-to\-end, runnable notebook examples that demonstrate how to use a TensorFlow or PyTorch training script with the SageMaker model parallelism library, see [Amazon SageMaker Distributed Training Notebook Examples](distributed-training-notebook-examples.md)\.

**Topics**
+ [Modify a TensorFlow Training Script](model-parallel-customize-training-script-tf.md)
+ [Modify a PyTorch Training Script](model-parallel-customize-training-script-pt.md)