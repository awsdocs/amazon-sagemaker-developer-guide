# SageMaker's Distributed Data Parallel Library<a name="data-parallel"></a>

**Important**  
To use new features with an existing notebook instance or Studio application, restart the notebook instance or Studio application to get the latest updates\. 

SageMaker's distributed data parallel library extends SageMaker’s training capabilities on deep learning models with near\-linear scaling efficiency, achieving fast time\-to\-train with minimal code changes\. 
+  The library optimizes your training job for AWS network infrastructure and Amazon EC2 instance topology\. 
+  The library takes advantage of gradient updates to communicate between nodes with a custom `AllReduce` algorithm\. 

When training a model on a large amount of data, machine learning practitioners often turn to distributed training to reduce the time to train\. In some cases, where time is of the essence, the business requirement is to finish training as quickly as possible or at least within a constrained time period\. Then, distributed training is scaled to use a cluster of multiple nodes—not just multiple GPUs in a computing instance, but multiple instances with multiple GPUs\. As the cluster size increases, so does the significant drop in performance\. This drop in performance is primarily caused by the communications overhead between nodes in a cluster\.  

SageMaker's distributed library offers two options for distributed training: model parallel and data parallel\. This guide focuses on how to train models using a data parallel strategy\. For more information on training with a model\-parallel strategy, refer to [SageMaker's Distributed Model Parallel](model-parallel.md)\. 

**Topics**
+ [Introduction to SageMaker's Distributed Data Parallel Library](data-parallel-intro.md)
+ [Use the SageMaker Distributed Data Parallel API](data-parallel-use-api.md)
+ [Modify Your Training Script using the SageMaker Data Parallel Library](data-parallel-modify-sdp.md)
+ [SageMaker distributed data parallel Configuration Tips and Pitfalls](data-parallel-config.md)
+ [Data Parallel Library FAQ](data-parallel-faq.md)