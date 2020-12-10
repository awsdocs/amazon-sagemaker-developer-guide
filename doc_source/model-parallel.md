# SageMaker distributed model parallel<a name="model-parallel"></a>

**Important**  
To use new features with an existing notebook instance or Studio app, you will need to restart it to get the latest updates\. 

Amazon SageMaker distributed model parallel \(SMP\) is a model parallelism library for training large deep learning models that were previously difficult to train due to GPU memory limitations\. SMP automatically and efficiently splits a model across multiple GPUs and instances and coordinates model training, allowing you to increase prediction accuracy by creating larger models with more parameters\.

You can use SMP to automatically partition your existing TensorFlow and PyTorch workloads across multiple GPUs with minimal code changes\. The SMP API can be accessed through the Amazon SageMaker SDK\.

Use the following sections to learn more about the model parallelism and the SMP library\.

**Topics**
+ [Introduction to Model Parallelism](model-parallel-intro.md)
+ [Core Features of SageMaker distributed model parallel](model-parallel-core-features.md)
+ [Use the SageMaker distributed model parallel API](model-parallel-use-api.md)
+ [Modify Your Training Script to Use SMP](model-parallel-customize-training-script.md)
+ [Configuration Tips and Pitfalls](model-parallel-customize-tips-pitfalls.md)