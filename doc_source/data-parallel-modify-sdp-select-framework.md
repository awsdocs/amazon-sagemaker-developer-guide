# Step 1: Modify Your Own Training Script<a name="data-parallel-modify-sdp-select-framework"></a>

Use this section to learn how to customize your training script to use the core features of the Amazon SageMaker distributed data parallel library\. To use the library\-specific API functions and parameters, we recommend you use this documentation alongside the [SageMaker data parallel library APIs](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html) in the *SageMaker Python SDK documentation*\.

The training script examples provided in these sections are simplified and designed to highlight the required changes you must make to use the library\. For end\-to\-end, runnable notebook examples that demonstrate how to use a TensorFlow or PyTorch training script with the SageMaker distributed data parallel library, see [Amazon SageMaker Distributed Training Notebook Examples](distributed-training-notebook-examples.md)\.

**Topics**
+ [Modify a TensorFlow Training Script](data-parallel-modify-sdp-tf2.md)
+ [Modify a PyTorch Training Script](data-parallel-modify-sdp-pt.md)
+ [Modify a PyTorch Lightning Script](data-parallel-modify-sdp-pt-lightning.md)