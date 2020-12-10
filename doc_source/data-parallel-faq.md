# Data parallelism FAQ<a name="data-parallel-faq"></a>
+  When using SageMaker distributed data parallelism \(SDP\), how are the allreduce\-supporting CPU instances managed? Do I have to create heterogenous CPU\-GPU clusters, or is the SageMaker service creating extra C5s for SDP\-enabled jobs? 

   SDP uses the CPUs available with GPU instances\. No additional C5 or CPU instances are launched i\.e\., if your SageMaker training job is 8 node ml\.p3dn\.24xlarge cluster, only 8 ml\.p3dn\.24xlarge instances are used, no additional instances are provisioned\.  
+  I have a training job taking 5 days on a single ml\.p3\.24xlarge instance with a set of hyperparameters H1 \(learning rate, batch size, optimizer, etc\)\. Is enabling SDP and using a 5x bigger cluster enough to experience an approximate 5x speedup? Or will I have to revisit its training hyperparameters after enabling SDP? 

   Enabling SDP, would change the overall batch size\. The new overall batchsize will be scaled linearly with the number of training instances used\. As a result of this, hyperparameters such as learning rate will have to be changed to ensure convergence\. 
+  Does SDP support Spot? 

   Yes\. SM managed spot training should be able to handle SDP\. User will have to specify path to checkpoint file in the SM training job\. User will also have to enable save/restore checkpoints in their training script as mentioned in the getting started doc\. 
+ What is the difference between SDP BFB and PyTorch Distributed Data Parallel \(DDP\) “Gradient Buckets”? From the  [PyTorch DDP documentation](https://pytorch.org/docs/master/notes/ddp.html): “To improve communication efficiency, the `Reducer` organizes parameter gradients into buckets, and reduces one bucket at a time\. Bucket size can be configured by setting the bucket\_cap\_mb argument in DDP constructor\. The mapping from parameter gradients to buckets is determined at the construction time, based on the bucket size limit and parameter sizes\. Model parameters are allocated into buckets in \(roughly\) the reverse order of `Model.parameters()` from the given model\. The reason for using the reverse order is because DDP expects gradients to become ready during the backward pass in approximately that order” 

   Primary difference is \- DDP’s fusion buffer is used for all\-reduce\. SDP’s BFB is used for parameter server style synchronization\. SDP is the first framework to shard fusion buffers in parameter server based gradient synchronization\.  
+ Is SDP relevant in single\-host, multi\-device setup?

   SDP can be used with single\-host, multi\-device setup\. However, SDP’s value \(faster and cheaper distributed deep learning model training\) is highly applicable in 2 or more node training clusters\. In 2 or more nodes, SDP’s innovative all\-reduce operation gives you significant performance improvement\. Also, on a single host, NVLink already contributes to in\-node all\-reduce efficiency\. 
+ Can SDP be used with PyTorch Lightning? 

   No\. However, with SDP’s DDP for PyTorch, you can write custom DDP to achieve the functionality\. 
+ Where should training dataset be stored? 

   The training dataset can be stored on an S3 bucket, or on an FSx drive\. See this [document for various supported input filesystem for a training job](https://sagemaker.readthedocs.io/en/stable/api/utility/inputs.html#sagemaker.inputs.FileSystemInput)\. 
+  With SageMaker SDP, is it mandatory to have training data in FSx for Lustre? Can EFS and S3 be used? 

   FSx is just a recommendation, to cut down the time taken to kick start the training\. It is not mandatory\. 
+ Can SDP be used with CPU nodes? 

   No\. SDP supports ml\.p3\.16xlarge, ml\.p3dn\.24xlarge, ml\.p4d\.24xlarge instances at this time\. 
+  What frameworks and framework versions are currently supported by SageMaker SDP at launch? 

   SDP currently supports PyTorch v1\.6 and Tensorflow v2\.3\.1\. Support for Tensorflow 1\.x\.  