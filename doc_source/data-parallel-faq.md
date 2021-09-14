# Data Parallel Library FAQ<a name="data-parallel-faq"></a>

Use the following to find answers to commonly asked questions about SageMaker's data parallelism library\.

**Q: When using the library, how are the `allreduce`\-supporting CPU instances managed? Do I have to create heterogeneous CPU\-GPU clusters, or does the SageMaker service create extra C5s for jobs that use the library? **

The library uses the CPUs available with GPU instances\. No additional C5 or CPU instances are launched; if your SageMaker training job is 8 node `ml.p3dn.24xlarge` clusters, only 8 `ml.p3dn.24xlarge` instances are used\. No additional instances are provisioned\.  

**Q: I have a training job taking 5 days on a single `ml.p3.24xlarge` instance with a set of hyperparameters H1 \(learning rate, batch size, optimizer, etc\)\. Is enabling SDP and using a 5x bigger cluster enough to experience an approximate 5x speedup? Or will I have to revisit its training hyperparameters after enabling SDP? **

Enabling the library would change the overall batch size\. The new overall batch size is scaled linearly with the number of training instances used\. As a result of this, hyperparameters, such as learning rate, have to be changed to ensure convergence\. 

**Q: Does the library support Spot? **

Yes\. You can use managed spot training\. You specify the path to the checkpoint file in the SageMaker training job\. You enable save and restore checkpoints in their training script as mentioned in the last step of [Script Modification Overview](data-parallel-modify-sdp.md#data-parallel-modify-sdp-overview)\. 

**Q: What is the difference between the library's balanced fusion buffers and PyTorch Distributed Data Parallel \(DDP\) “Gradient Buckets”? **

The primary difference is that DDP’s fusion buffer is used for `AllReduce` and the library's balanced fusion buffer is used for parameter server style synchronization\. The library is the first framework to shard fusion buffers in parameter server\-based gradient synchronization\.  

From the  [PyTorch DDP documentation](https://pytorch.org/docs/master/notes/ddp.html): “To improve communication efficiency, the `Reducer` organizes parameter gradients into buckets, and reduces one bucket at a time\. Bucket size can be configured by setting the `bucket_cap_mb` argument in DDP constructor\. The mapping from parameter gradients to buckets is determined at the construction time, based on the bucket size limit and parameter sizes\. Model parameters are allocated into buckets in \(roughly\) the reverse order of `Model.parameters()` from the given model\. The reason for using the reverse order is because DDP expects gradients to become ready during the backward pass in approximately that order\.” 

**Q: Is the library relevant in single\-host, multi\-device setup?**

The library can be used with a single\-host, multi\-device setup\. However, in two or more nodes, the library’s `AllReduce` operation gives you significant performance improvement\. Also, on a single host, NVLink already contributes to in\-node `AllReduce` efficiency\.

**Q: Can the library be used with PyTorch Lightning?**

No\. However, with the library’s DDP for PyTorch, you can write custom DDP to achieve the functionality\. 

**Q: Where should the training dataset be stored? **

The training dataset can be stored in an Amazon S3 bucket or on an Amazon FSx drive\. See this [document for various supported input filesystem for a training job](https://sagemaker.readthedocs.io/en/stable/api/utility/inputs.html#sagemaker.inputs.FileSystemInput)\. 

**Q: When using the library, is it mandatory to have training data in FSx for Lustre? Can Amazon EFS and Amazon S3 be used? **

We recommend using Amazon FSx to cut down the time to kickstart the training\. It is not mandatory\. 

**Q: Can the library be used with CPU nodes?** 

No\. The library supports `ml.p3.16xlarge`, `ml.p3dn.24xlarge`, and `ml.p4d.24xlarge` instances at this time\. 

**Q: What frameworks and framework versions are currently supported by the library at launch?** 

The library currently supports PyTorch v1\.6\.0 or later and Tensorflow v2\.3\.0 or later\. It doesn't support Tensorflow 1\.x\. For more information about which version of the library is packaged within AWS deep learning containers, see [Release Notes for Deep Learning Containers](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/dlc-release-notes.html)\.

**Q: Does the library support AMP?**

Yes, SageMaker's distributed data parallelism library supports Automatic Mixed Precision \(AMP\) out of the box\. No extra action is needed to enable AMP other than the framework\-level modifications to your training script\. If gradients are in FP16, the SageMaker data parallelism library runs its `AllReduce` operation in FP16\. For more information about implementing AMP APIs to your training script, see the following resources:
+ [Frameworks \- PyTorch](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html#pytorch) in the *NVIDIA Deep Learning Performace documentation*
+ [Frameworks \- TensorFlow](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html#tensorflow) in the *NVIDIA Deep Learning Performace documentation*
+ [Automatic Mixed Precision for Deep Learning](https://developer.nvidia.com/automatic-mixed-precision) in the *NVIDIA Developer Docs*
+ [Introducing native PyTorch automatic mixed precision for faster training on NVIDIA GPUs](https://pytorch.org/blog/accelerating-training-on-nvidia-gpus-with-pytorch-automatic-mixed-precision/) in the *PyTorch Blog*
+ [TensorFlow mixed precision APIs](https://www.tensorflow.org/guide/mixed_precision) in the *TensorFlow documentation*

**Q: How do I identify if my distributed training job is slowed down due to I/O bottleneck?**

With a larger cluster, the training job requires more I/O throughput, and therefore the training throughput might take longer \(more epochs\) to ramp up to the maximum performance\. This indicates that I/O is being bottlenecked and cache is harder to build up as you scale nodes up \(higher throughput requirement, more complex network topology\)\. For more information about monitoring the Amazon FSx throughput on CloudWatch, see [Monitoring FSx for Lustre](https://docs.aws.amazon.com/fsx/latest/LustreGuide/monitoring_overview.html) in the *FSx for Lustre User Guide*\. 

**Q: How do I resolve I/O bottlenecks when running a distributed training job with data parallelism?**

It is highly recommended to use Amazon FSx as your data channel if you are using Amazon S3\. If you are already using Amazon FSx but still having I/O bottleneck problems, you might have set up your Amazon FSx file system with a low I/O throughput and a small storage capacity\. For more information about how to estimate and choose the right size of I/O throughput capacity, see [Use Amazon FSx and set up sufficient storage and throughput capacity](http://Use FSx and set up sufficient storage and throughput capacity)\.