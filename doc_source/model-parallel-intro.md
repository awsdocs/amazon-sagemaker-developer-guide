# Introduction to Model Parallelism<a name="model-parallel-intro"></a>

**Model parallelism** is the process of splitting a model up between multiple devices or nodes \(such as GPU\-equipped instances\) and creating an efficient pipeline to train the model across these devices to maximize GPU utilization\. 

Use the sections on this page to learn more about model parallelism, including how it can be used to overcome issues that arise when training large ML models, and important considerations when integrating it into your ML training script\. 

## What is Model Parallelism?<a name="model-parallel-what-is"></a>

Increasing deep learning model size \(layers and parameters\) can result in better accuracy\. However, there is a limit to the maximum model size you can fit in a single GPU\. When training deep learning models, GPU memory limitations can be a bottleneck in the following ways:
+ They can limit the size of the model you train\. Given that larger models tend to achieve higher accuracy, this directly translates to trained model accuracy\.
+ They can limit the batch size you train with, leading to lower GPU utilization and slower training\.

To overcome the limitations associated with training a model on a single GPU, you can use model parallelism to distribute and train your model on multiple computing devices\. 

## Important Considerations when Using Model Parallelism<a name="model-parallel-important-considerations"></a>

When you use model parallelism, you must consider the following:
+ **How you split your model across devices**: The computational graph of your model, sizes of model parameters and activations, and your resource constraints \(for example, time vs\. memory\) determine the best partitioning strategy\.

  To reduce the time and effort required to efficiently split your model, you can use automated model splitting features offered by Amazon SageMaker's distributed model parallel library\.
+ **Achieving parallelization**: Model training—that is, forward computations and backward propagation—is inherently sequential, where each operation must wait for its inputs to be computed by another operation\. For this reason, forward and backward pass stages of deep learning training are not easily parallelizable, and naively splitting a model across multiple GPUs may lead to poor device utilization\. For example, a layer on GPU `i+1` has to wait for the output from a layer on GPU `i`, and so GPU `i+1` remains idle during this waiting period\.

  The model parallel library can achieve true parallelization by implementing pipelined execution by building an efficient computation schedule where different devices can work on forward and backward passes for different data samples at the same time\.

To learn how you can use the library to efficiently split your model across devices and improve device utilization during training, see [Core Features of the SageMaker Model Parallel Library](model-parallel-core-features.md)\.