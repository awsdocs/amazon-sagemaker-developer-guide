# Introduction to SageMaker's Distributed Data Parallel Library<a name="data-parallel-intro"></a>

## Why Use SageMaker Distributed Data Parallel Library?<a name="data-parallel-intro-why"></a>

SageMaker's distributed data parallel library addresses communications overhead in two ways:

1. The library performs `AllReduce`, a key operation during distributed training that is responsible for a large portion of communication overhead\.

1. The library performs optimized node\-to\-node communication by fully utilizing AWS’s network infrastructure and Amazon EC2 instance topology\. 

Use this data parallel library to increase speed by up to 25% in training models such as BERT\. While implementations like Horovod offer sub\-linear performance at scale, this library offers near\-linear performance at scale\. This means that you get a faster training time and a lower cost to train a model\.

## Training Benchmarks<a name="data-parallel-benchmarks"></a>

 **PyTorch with SageMaker's data parallel library** 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/data-parallel/sdp-pytorch.png) 

 Using instance type `p3dn.24xl` and on 2, 4, and 8 node clusters: 
+ *BERT*: When used with PyTorch, the SageMaker library is 41%, 52%, and 13% faster than PyTorch\-DDP\.
+ *MaskRCNN*: When used with PyTorch, the SageMaker library is 4%, 19%, and 15% faster than PyTorch\-DDP\.

These benchmarks were run on PyTorch v1\.6 using `ml.p3dn.24xlarge` instances\. You can find the training code on the [SageMaker examples website](https://github.com/aws/amazon-sagemaker-examples/tree/master/training/distributed_training/pytorch/data_parallel)\. The examples website also has benchmark training code for these models [using TensorFlow 2\.3](https://github.com/aws/amazon-sagemaker-examples/tree/master/training/distributed_training/tensorflow/data_parallel)\. 

## Optimal Bandwidth Use with Balanced Fusion Buffer<a name="data-parallel-bfb"></a>

SageMaker's distributed data parallel library uses a communication pattern similar to parameter servers to reduce the amount of data transferred and the number of steps involved in averaging gradients from multiple GPUs\. It also uses a new technique called balanced fusion buffers to make optimal use of the bandwidth available across all nodes in the cluster\. 

One key disadvantage of traditional parameter servers is their suboptimal use of available network bandwidth\. Parameter servers treat variables as atomic units and place each variable on one server\. Since gradients become available sequentially during the backward pass, at any given instant, there is imbalance in the volume of data being sent and received from different servers\. Some servers are receiving and sending more data, some less, and some none\. This problem becomes worse as the number of parameter servers increases\. 

The library addresses these problems by introducing *balanced fusion buffers*\. A balanced fusion buffer is a buffer in the GPU that holds the gradients until the size of the buffer exceeds a threshold\. In a setup with N parameter servers, when the buffer exceeds the threshold, the balanced fusion buffer is copied to CPU memory, sharded into N parts, and the ith part is sent to the ith parameter server\. Each server receives exactly the same number of bytes from a balanced fusion buffer\. The ith server receives the ith partition of the balanced fusion buffer from all workers, sums them up, and sends the results back to all workers\. Since all the servers participate equally in averaging each balanced fusion buffer, server bandwidth is efficiently utilized\. 

## Optimal GPU Usage with Efficient `AllReduce` Overlapping with a Backward Pass<a name="data-parallel-allreduce"></a>

SageMaker's distributed data parallel library achieves optimal overlapping of the `AllReduce` operation with the backward pass, significantly improving the GPU utilization, and achieves near\-linear scaling efficiency and faster time to train by optimizing tasks between CPUs and GPUs\. The library performs `AllReduce` in parallel while GPU is computing gradients without taking away additional GPU cycles, which makes the library faster\.
+  *Leverages CPUs*: The library uses CPUs to AllReduce gradients, offloading this task from the GPUs\. 
+ * Improved GPU usage*: The cluster’s GPUs focus on computing gradients, improving their utilization throughout training\.