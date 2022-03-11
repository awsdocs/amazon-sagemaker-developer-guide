# SageMaker's Distributed Model Parallel<a name="model-parallel"></a>

Use Amazon SageMaker's distributed model parallel library to train large deep learning \(DL\) models that are difficult to train due to GPU memory limitations\. The library automatically and efficiently splits a model across multiple GPUs and instances\. Using the library, you can achieve a target prediction accuracy faster by efficiently training larger DL models with billions or trillions of parameters\.

You can use the library to automatically partition your own TensorFlow and PyTorch models across multiple GPUs and multiple nodes with minimal code changes\. You can access the library's API through the SageMaker Python SDK\.

Use the following sections to learn more about model parallelism and the SageMaker model parallel library\. This library's API documentation is located at [Distributed Training APIs](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html) in the *SageMaker Python SDK documentation*\. 

To track the latest updates of the library, see the [SageMaker Distributed Model Parallel Release Notes](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_release_notes/smd_model_parallel_change_log.html) in the *SageMaker Python SDK documentation*\.

**Topics**
+ [Introduction to Model Parallelism](model-parallel-intro.md)
+ [Core Features of the SageMaker Model Parallel Library](model-parallel-core-features.md)
+ [Run a SageMaker Distributed Training Job with Model Parallelism](model-parallel-use-api.md)
+ [Extended Features of the SageMaker Model Parallel Library for PyTorch](model-parallel-extended-features-pytorch.md)
+ [SageMaker Distributed Model Parallel Best Practices](model-parallel-best-practices.md)
+ [SageMaker Distributed Model Parallel Configuration Tips and Pitfalls](model-parallel-customize-tips-pitfalls.md)
+ [Model Parallel Troubleshooting](distributed-troubleshooting-model-parallel.md)