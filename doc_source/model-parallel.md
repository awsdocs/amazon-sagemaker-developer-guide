# SageMaker's Distributed Model Parallel<a name="model-parallel"></a>

**Important**  
To use new features with an existing notebook instance or Studio app, restart it to get the latest updates\. 

Amazon SageMaker's distributed model parallel library \(the library\) can be used to training large deep learning models that were previously difficult to train due to GPU memory limitations\. The library automatically and efficiently splits a model across multiple GPUs and instances and coordinates model training, allowing you to increase prediction accuracy by creating larger models with more parameters\.

You can use the library to automatically partition your existing TensorFlow and PyTorch workloads across multiple GPUs with minimal code changes\. You can access the library's API through the SageMaker SDK\.

Use the following sections to learn more about model parallelism and the SageMaker model parallel library\. This library's API documentation is located in the SageMaker Python SDK under [Distributed Training APIs](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\. To see the latest updates to the library, refer to the [release notes](https://github.com/aws/sagemaker-python-sdk/tree/master/doc/api/training/smd_model_parallel_release_notes)\.

**Topics**
+ [Introduction to Model Parallelism](model-parallel-intro.md)
+ [Core Features of SageMaker Distributed Model Parallel](model-parallel-core-features.md)
+ [Use the SageMaker Distributed Model Parallel API](model-parallel-use-api.md)
+ [Modify Your Training Script Using SageMaker's Distributed Model Parallel Library](model-parallel-customize-training-script.md)
+ [SageMaker distributed model parallel Configuration Tips and Pitfalls](model-parallel-customize-tips-pitfalls.md)