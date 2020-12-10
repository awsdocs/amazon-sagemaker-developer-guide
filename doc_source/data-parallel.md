# SageMaker distributed data parallel<a name="data-parallel"></a>

**Important**  
To use new features with an existing notebook instance or Studio app, you will need to restart it to get the latest updates\. 

 SageMaker distributed data parallel \(SDP\) extends SageMaker’s training capabilities on deep learning models with near\-linear scaling efficiency, achieving fast time\-to\-train with minimal code changes\. 
+  SDP optimizes your training job for AWS network infrastructure and EC2 instance topology\. 
+  SDP takes advantage of gradient update to communicate between nodes with a custom `AllReduce` algorithm\. 

 When training a model on a large amount of data, machine learning practitioners will often turn to distributed training to reduce the time to train\. In some cases, where time is of the essence, the business requirement is to finish training as quickly as possible or at least within a constrained time period\. Then, distributed training is scaled to use a cluster of multiple nodes, meaning not just multiple GPUs in a computing instance, but multiple instances with multiple GPUs\. As the cluster size increases, so does the significant drop in performance\. This drop in performance is primarily caused the communications overhead between nodes in a cluster\.  

 SageMaker distributed \(SMD\) offers two options for distributed training: SageMaker model parallel \(SMP\) and SageMaker data parallel \(SDP\)\. This guide focuses on how to train models using a data parallel strategy\. For more information on training with a model parallel strategy, refer to [SageMaker distributed model parallel](model-parallel.md)\. 

**Topics**
+ [Introduction to SageMaker data parallel](data-parallel-intro.md)
+ [Use the SageMaker distributed data parallel API](data-parallel-use-api.md)
+ [Modify your training script for SDP](data-parallel-modify-sdp.md)
+ [Data parallelism FAQ](data-parallel-faq.md)