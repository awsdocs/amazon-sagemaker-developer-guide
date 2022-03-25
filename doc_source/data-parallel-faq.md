# Amazon SageMaker Data Parallel Library FAQ<a name="data-parallel-faq"></a>

Use the following to find answers to commonly asked questions about SageMaker's data parallelism library\.

**Q: When using the library, how are the `allreduce`\-supporting CPU instances managed? Do I have to create heterogeneous CPU\-GPU clusters, or does the SageMaker service create extra C5s for jobs that use the library? **

The library uses the CPUs available with GPU instances\. No additional C5 or CPU instances are launched; if your SageMaker training job is 8\-node `ml.p3dn.24xlarge` clusters, only 8 `ml.p3dn.24xlarge` instances are used\. No additional instances are provisioned\.  

**Q: I have a training job taking 5 days on a single `ml.p3.24xlarge` instance with a set of hyperparameters H1 \(learning rate, batch size, optimizer, etc\)\. Is using SageMaker's data parallelism library and a five\-time bigger cluster enough to achieve an approximate five\-time speedup? Or do I have to revisit its training hyperparameters after activating the library? **

The library changes the overall batch size\. The new overall batch size is scaled linearly with the number of training instances used\. As a result of this, hyperparameters, such as learning rate, have to be changed to ensure convergence\. 

**Q: Does the library support Spot? **

Yes\. You can use managed spot training\. You specify the path to the checkpoint file in the SageMaker training job\. You save and restore checkpoints in their training script as mentioned in the last steps of [Modify a TensorFlow Training Script](data-parallel-modify-sdp-tf2.md) and [Modify a PyTorch Training Script](data-parallel-modify-sdp-pt.md)\. 

**Q: Is the library relevant in a single\-host, multi\-device setup?**

The library can be used in single\-host multi\-device training but the library offers performance improvements only in multi\-host training\.

**Q: Can the library be used with PyTorch Lightning?**

No\. However, with the library’s DDP for PyTorch, you can write custom DDP to achieve the functionality\. 

**Q: Where should the training dataset be stored? **

The training dataset can be stored in an Amazon S3 bucket or on an Amazon FSx drive\. See this [document for various supported input file systems for a training job](https://sagemaker.readthedocs.io/en/stable/api/utility/inputs.html#sagemaker.inputs.FileSystemInput)\. 

**Q: When using the library, is it mandatory to have training data in FSx for Lustre? Can Amazon EFS and Amazon S3 be used? **

We generally recommend you use Amazon FSx because of its lower latency and higher throughput\. If you prefer, you can use Amazon EFS or Amazon S3\.

**Q: Can the library be used with CPU nodes?** 

No\. The library supports `ml.p3.16xlarge`, `ml.p3dn.24xlarge`, and `ml.p4d.24xlarge` instances at this time\. 

**Q: What frameworks and framework versions are currently supported by the library at launch?** 

The library currently supports PyTorch v1\.6\.0 or later and TensorFlow v2\.3\.0 or later\. It doesn't support TensorFlow 1\.x\. For more information about which version of the library is packaged within AWS deep learning containers, see [Release Notes for Deep Learning Containers](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/dlc-release-notes.html)\.

**Q: Does the library support AMP?**

Yes, SageMaker's distributed data parallelism library supports Automatic Mixed Precision \(AMP\) out of the box\. No extra action is needed to use AMP other than the framework\-level modifications to your training script\. If gradients are in FP16, the SageMaker data parallelism library runs its `AllReduce` operation in FP16\. For more information about implementing AMP APIs to your training script, see the following resources:
+ [Frameworks \- PyTorch](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html#pytorch) in the *NVIDIA Deep Learning Performace documentation*
+ [Frameworks \- TensorFlow](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html#tensorflow) in the *NVIDIA Deep Learning Performace documentation*
+ [Automatic Mixed Precision for Deep Learning](https://developer.nvidia.com/automatic-mixed-precision) in the *NVIDIA Developer Docs*
+ [Introducing native PyTorch automatic mixed precision for faster training on NVIDIA GPUs](https://pytorch.org/blog/accelerating-training-on-nvidia-gpus-with-pytorch-automatic-mixed-precision/) in the *PyTorch Blog*
+ [TensorFlow mixed precision APIs](https://www.tensorflow.org/guide/mixed_precision) in the *TensorFlow documentation*

**Q: How do I identify if my distributed training job is slowed down due to I/O bottleneck?**

With a larger cluster, the training job requires more I/O throughput, and therefore the training throughput might take longer \(more epochs\) to ramp up to the maximum performance\. This indicates that I/O is being bottlenecked and cache is harder to build up as you scale nodes up \(higher throughput requirement and more complex network topology\)\. For more information about monitoring the Amazon FSx throughput on CloudWatch, see [Monitoring FSx for Lustre](https://docs.aws.amazon.com/fsx/latest/LustreGuide/monitoring_overview.html) in the *FSx for Lustre User Guide*\. 

**Q: How do I resolve I/O bottlenecks when running a distributed training job with data parallelism?**

We highly recommend that you use Amazon FSx as your data channel if you are using Amazon S3\. If you are already using Amazon FSx but still having I/O bottleneck problems, you might have set up your Amazon FSx file system with a low I/O throughput and a small storage capacity\. For more information about how to estimate and choose the right size of I/O throughput capacity, see [Use Amazon FSx and set up an optimal storage and throughput capacity](data-parallel-config.md#data-parallel-config-fxs)\.

**Q: \(For the library v1\.4\.0 or later\) How do I resolve the `Invalid backend` error while initializing process group\.**

If you encounter the error message `ValueError: Invalid backend: 'smddp'` when calling `init_process_group`, this is due to the breaking change in the library v1\.4\.0 and later\. You must import the PyTorch client of the library, `smdistributed.dataparallel.torch.torch_smddp`, which registers `smddp` as a backend for PyTorch\. To learn more, see [Use the SageMaker Distributed Data Parallel Library as the Backend of `torch.distributed`](data-parallel-modify-sdp-pt.md#data-parallel-enable-smddp-backend)\.

**Q: \(For the library v1\.4\.0 or later\) I would like to call the collective primitives of the [torch\.distributed](https://pytorch.org/docs/stable/distributed.html) interface\. Which primitives does the `smddp` backend support?**

In v1\.4\.0, the library supports `all_reduce`, `broadcast`, `reduce`, `all_gather`, and `barrier`\.

**Q: \(For the library v1\.4\.0 or later\) Does this new API work with other custom DDP classes or libraries like Apex DDP? **

The SageMaker data parallel library is tested with other third\-party distributed data parallel libraries and framework implementations that use the `torch.distribtued` modules\. Using the SageMaker data parallel library with custom DDP classes works as long as the collectives used by the custom DDP classes are supported by the library\. See the preceding question for a list of supported collectives\. If you have these use cases and need further support, reach out to the SageMaker team through the [AWS Support Center](https://console.aws.amazon.com/support/) or [AWS Developer Forums for Amazon SageMaker](https://forums.aws.amazon.com/forum.jspa?forumID=285)\.

**Q: Does the library support the bring\-your\-own\-container \(BYOC\) option? If so, how do I install the library and run a distributed training job by writing a custom Dockerfile?**

If you want to integrate the SageMaker data parallel library and its minimum dependencies in your own Docker container, BYOC is the right approach\. You can build your own container using the binary file of the library\. The recommended process is to write a custom Dockerfile with the library and its dependencies, build the Docker container, host it in Amazon ECR, and use the ECR image URI to launch a training job using the SageMaker generic estimator class\. For more instructions on how to prepare a custom Dockerfile for distributed training in SageMaker with the SageMaker data parallel library, see [Create Your Own Docker Container with the SageMaker Distributed Data Parallel Library](data-parallel-use-api.md#data-parallel-bring-your-own-container)\.