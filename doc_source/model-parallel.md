# SageMaker's Distributed Model Parallel<a name="model-parallel"></a>

**Important**  
To use new features with an existing notebook instance or Studio app, restart it to get the latest updates\. 

Use Amazon SageMaker's distributed model parallel library to train large deep learning \(DL\) models that are difficult to train due to GPU memory limitations\. The library automatically and efficiently splits a model across multiple GPUs and instances\. Using the library, you can achieve a target prediction accuracy faster by efficiently training larger DL models with billions or trillions of parameters\.

You can use the library to automatically partition your own TensorFlow and PyTorch models across multiple GPUs and multiple nodes with minimal code changes\. You can access the library's API through the SageMaker Python SDK\.

Use the following sections to learn more about model parallelism and the SageMaker model parallel library\. This library's API documentation is located at [Distributed Training APIs](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html) in the *SageMaker Python SDK documentation*\. To see the latest updates to the library, refer to the [release notes](https://github.com/aws/sagemaker-python-sdk/blob/master/doc/api/training/smd_model_parallel_release_notes/smd_model_parallel_change_log.md)\.

**Topics**
+ [Introduction to Model Parallelism](model-parallel-intro.md)
+ [Core Features of SageMaker Distributed Model Parallel](model-parallel-core-features.md)
+ [Modify Your Training Script Using SageMaker's Distributed Model Parallel Library](model-parallel-customize-training-script.md)
+ [Run a SageMaker Distributed Model Parallel Training Job](model-parallel-use-api.md)
+ [SageMaker distributed model parallel Configuration Tips and Pitfalls](model-parallel-customize-tips-pitfalls.md)
+ [Model Parallel Troubleshooting](distributed-troubleshooting-model-parallel.md)