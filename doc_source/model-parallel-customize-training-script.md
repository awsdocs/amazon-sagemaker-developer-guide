# Modify Your Training Script to Use SageMaker's Distributed Model Parallel Library<a name="model-parallel-customize-training-script"></a>

Use this section to learn how to customize your training script with Amazon SageMaker's distributed model parallel library\-specific API functions and parameters\. We recommend you use this documentation alongside the [model parallel library API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\.

The training script examples provided in these sections are simplified and designed to highlight the required changes you must make to use the library\. For end to end, runnable notebook examples that demonstrate how to use a TensorFlow or PyTorch training script with the SageMaker distributed model parallel library, see [Distributed Training Jupyter Notebook Examples](distributed-training-notebook-examples.md)\.

**Topics**
+ [Modify a TensorFlow Training Script](model-parallel-customize-training-script-tf.md)
+ [Modify a PyTorch Training Script](model-parallel-customize-training-script-pt.md)