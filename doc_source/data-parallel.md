# SageMaker's Distributed Data Parallel Library<a name="data-parallel"></a>

The SageMaker distributed data parallel library extends SageMaker training capabilities on deep learning models with near\-linear scaling efficiency, achieving fast time\-to\-train with minimal code changes\. 

When training a model on a large amount of data, machine learning practitioners often turn to distributed training to reduce the time to train\. In some cases, where time is of the essence, the business requirement is to finish training as quickly as possible or at least within a constrained time period\. Then, distributed training is scaled to use a cluster of multiple nodes—not just multiple GPUs in a computing instance, but multiple instances with multiple GPUs\. As the cluster size increases, so does the significant drop in performance\. This drop in performance is primarily caused by the communications overhead between nodes in a cluster\.

To resolve such overhead problems, SageMaker offers two distributed training options: SageMaker model parallel and SageMaker data parallel\. This guide focuses on how to train models using the SageMaker data parallel library\. 
+  The library optimizes your training job for AWS network infrastructure and Amazon EC2 instance topology\. 
+  The library takes advantage of gradient updates to communicate between nodes with a custom `AllReduce` algorithm\. 

To track the latest updates of the library, see the [SageMaker Distributed Data Parallel Release Notes](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel_release_notes/smd_data_parallel_change_log.html) in the *SageMaker Python SDK documentation*\.

For more information about training with a model\-parallel strategy, see [SageMaker's Distributed Model Parallel](model-parallel.md)\.

**Topics**
+ [Introduction to SageMaker's Distributed Data Parallel Library](data-parallel-intro.md)
+ [Supported Frameworks, AWS Regions, and Instances Types](distributed-data-parallel-support.md)
+ [Run a SageMaker Distributed Training Job with Data Parallelism](data-parallel-modify-sdp.md)
+ [SageMaker Distributed Data Parallel Configuration Tips and Pitfalls](data-parallel-config.md)
+ [Amazon SageMaker Data Parallel Library FAQ](data-parallel-faq.md)
+ [Data Parallel Troubleshooting](distributed-troubleshooting-data-parallel.md)