# Introduction to SageMaker data parallel<a name="data-parallel-intro"></a>

## Why use SageMaker data parallel?<a name="data-parallel-intro-why"></a>

 SageMaker distributed data parallel \(SDP\) addresses communications overhead in two ways: 1\) SDP’s innovative technique to perform AllReduce, a key operation during distributed training that is responsible for a large portion of communication overhead; and 2\) SDP’s ability to perform optimized node\-to\-node communication by fully utilizing AWS’s network infrastructure and EC2 instance topology\. 

 Use SDP to achieve up to 25% speed up in training state\-of\-the art models such as BERT\. Whereas state\-of\-the\-art implementations like Horovod offer sub\-linear performance at scale, SDP offers near\-linear performance at scale\. This means that you get a faster training time and a lower cost to train a model\. 

## Training benchmarks<a name="data-parallel-benchmarks"></a>

 **PyTorch with SDP** 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/data-parallel/sdp-pytorch.png) 

 **Fig 1: SageMaker data parallel vs PyTorch DDP** 

 Using instance type p3dn\.24xl and on 2, 4, 8 node clusters: 
+ BERT: PyTorch\-SDP is 41%, 52%, 15% faster than PyTorch\-DDP
+ MaskRCNN: PyTorch\-SDP is 4%, 19%, 15% faster than PyTorch\-DDP

 **TensorFlow2 with SDP** 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/data-parallel/sdp-tf2.png) 

 **Fig 2: SageMaker data parallel vs TensorFlow2 with Horovod** 

 Using instance type p3dn\.24xl and on 2, 4, 8 node clusters: 
+ BERT: TensorFlow2\-SDP is 23%, 5%, 4% faster than TensorFlow2 with Horovod
+ MaskRCNN: TensorFlow2\-SDP is 8%, 2%, 6% faster than TensorFlow2 with Horovod

## Optimal bandwidth use with Balanced Fusion Buffer<a name="data-parallel-bfb"></a>

 SDP uses a communication pattern similar to parameter servers to reduce the amount of data transferred and the number of steps involved in averaging gradients from multiple GPUs\. It also uses a new technique called **Balanced Fusion Buffer **to make optimal use of the bandwidth available across all nodes in the cluster\. 

 One key disadvantage of traditional parameter servers is their suboptimal use of available network bandwidth\. Parameter servers treat variables as atomic units and place each variable on one server\. Since gradients become available sequentially during the backward pass, at any given instant there is imbalance in the volume of data being sent and received from different servers\. Some servers will be receiving and sending more data, some less and some none\. This problem becomes worse as the number of parameter servers increases\. 

 SDP addresses these problems by introducing *balanced fusion buffers*\. Balanced fusion buffer is a buffer in the GPU that holds the gradients until the size of the buffer exceeds a threshold\. In a setup with N parameter servers, when threshold exceeds, the BFB is copied to CPU memory, sharded into N parts and ith part is sent to ith parameter server\. Each server receives exactly the same number of bytes from a balanced fusion buffer\. ith server receives ith partition of the BFB from all workers, sums them up and sends the results back to all workers\. Since all the servers participate equally in averaging each balanced fusion buffer, server bandwidth is efficiently utilized\. 

## Optimal GPU usage with efficient all\-reduce overlapping with backward pass<a name="data-parallel-allreduce"></a>

 SDP achieves optimal overlapping of all\-reduce operation with the backward pass significantly improving the GPU utilization and hence achieves near\-linear scaling efficiency and achieves faster time to train\. This is achieved by optimizing tasks between CPUs and GPUs\. SDP performs all\-reduce in parallel while GPU is computing gradients without taking away additional GPU cycles which makes SDP faster\. 
+  Leverages CPUs: SDP uses CPUs to AllReduce gradients, offloading this task from the GPUs 
+  Improved GPU usage: the cluster’s GPUs focus on computing gradients, improving their utilization throughout training 