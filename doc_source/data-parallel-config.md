# SageMaker Distributed Data Parallel Configuration Tips and Pitfalls<a name="data-parallel-config"></a>

Review the following tips and pitfalls before using SageMaker's distributed data parallel library\. This list includes tips that are applicable across frameworks\.

**Topics**
+ [Data Preprocessing](#data-parallel-config-dataprep)
+ [Single Versus Multiple Nodes](#data-parallel-config-multi-node)
+ [Debug Scaling Efficiency with Debugger](#data-parallel-config-debug)
+ [Batch Size](#data-parallel-config-batch-size)
+ [Custom MPI Options](#data-parallel-config-mpi-custom)
+ [Use Amazon FSx and set up an optimal storage and throughput capacity](#data-parallel-config-fxs)

## Data Preprocessing<a name="data-parallel-config-dataprep"></a>

If you preprocess data during training using an external library that utilizes the CPU, you may run into a CPU bottleneck because SageMaker distributed data parallel uses the CPU for `AllReduce` operations\. You may be able to improve training time by moving preprocessing steps to a library that uses GPUs or by completing all preprocessing before training\.

## Single Versus Multiple Nodes<a name="data-parallel-config-multi-node"></a>

We recommend that you use this library with multiple nodes\. The library can be used with a single\-host, multi\-device setup \(for example, a single ML compute instance with multiple GPUs\); however, when you use two or more nodes, the library’s `AllReduce` operation gives you significant performance improvement\. Also, on a single host, NVLink already contributes to in\-node `AllReduce` efficiency\.

## Debug Scaling Efficiency with Debugger<a name="data-parallel-config-debug"></a>

You can use Amazon SageMaker Debugger to monitor and visualize CPU and GPU utilization and other metrics of interest during training\. You can use Debugger [built\-in rules](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) to monitor computational performance issues, such as `CPUBottleneck`, `LoadBalancing`, and `LowGPUUtilization`\. You can specify these rules with [Debugger configurations](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-configuration.html) when you define an Amazon SageMaker Python SDK estimator\. If you use AWS CLI and AWS Boto3 for training on SageMaker, you can enable Debugger as shown in [Configure Debugger Using Amazon SageMaker API](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-createtrainingjob-api.html)\.

To see an example using Debugger in a SageMaker training job, you can reference one of the notebook examples in the [SageMaker Notebook Examples GitHub repository](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger)\. To learn more about Debugger, see [Amazon SageMaker Debugger](https://docs.aws.amazon.com/sagemaker/latest/dg/train-debugger.html)\.

## Batch Size<a name="data-parallel-config-batch-size"></a>

In distributed training, as more nodes are added, batch sizes should increase proportionally\. To improve convergence speed as you add more nodes to your training job and increase the global batch size, increase the learning rate\.

One way to achieve this is by using a gradual learning rate warmup where the learning rate is ramped up from a small to a large value as the training job progresses\. This ramp avoids a sudden increase of the learning rate, allowing healthy convergence at the start of training\. For example, you can use a *Linear Scaling Rule* where each time the mini\-batch size is multiplied by k, the learning rate is also multiplied by k\. To learn more about this technique, see the research paper, [Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour](https://arxiv.org/pdf/1706.02677.pdf), Sections 2 and 3\.

## Custom MPI Options<a name="data-parallel-config-mpi-custom"></a>

The SageMaker distributed data parallel library employs Message Passing Interface \(MPI\), a popular standard for managing communication between nodes in a high\-performance cluster, and uses NVIDIA’s NCCL library for GPU\-level communication\. When you use the data parallel library with a TensorFlow or Pytorch `Estimator`, the respective container sets up the MPI environment and executes the `mpirun` command to start jobs on the cluster nodes\.

You can set custom MPI operations using the `custom_mpi_options` parameter in the `Estimator`\. Any `mpirun` flags passed in this field are added to the `mpirun` command and executed by SageMaker for training\. For example, you may define the `distribution` parameter of an `Estimator` using the following to use the [https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/env.html#nccl-debug](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/env.html#nccl-debug) variable to print the NCCL version at the start of the program:

```
distribution = {'smdistributed':{'dataparallel':{'enabled': True, "custom_mpi_options": "-verbose -x NCCL_DEBUG=VERSION"}}}
```

## Use Amazon FSx and set up an optimal storage and throughput capacity<a name="data-parallel-config-fxs"></a>

When training a model on multiple nodes with distributed data parallelism, it is highly recommended to use [FSx for Lustre](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html)\. Amazon FSx is a scalable and high\-performance storage service that supports shared file storage with a faster throughput\. Using Amazon FSx storage at scale, you can achieve a faster data loading speed across the compute nodes\.

Typically, with distributed data parallelism, you would expect that the total training throughput scales near\-linearly with the number of GPUs\. However, if you use suboptimal Amazon FSx storage, the training performance might slow down due to a low Amazon FSx throughput\. 

For example, if you use the [**SCRATCH\_2** deployment type of Amazon FSx file system](https://docs.aws.amazon.com/fsx/latest/LustreGuide/performance.html#fsx-aggregate-perf) with the minimum 1\.2 TiB storage capacity, the I/O throughput capacity is 240 MB/s\. Amazon FSx storage works in a way that you can assign physical storage devices, and the more devices assigned, the larger throughput you get\. The smallest storage increment for the SRATCH\_2 type is 1\.2 TiB, and the corresponding throughput gain is 240 MB/s\.

Assume that you have a model to train on a 4\-node cluster over a 100 GB data set\. With a given batch size that’s optimized to the cluster, assume that the model can complete one epoch in about 30 seconds\. In this case, the minimum required I/O speed is approximately 3 GB/s \(100 GB / 30 s\)\. This is apparently a much higher throughput requirement than 240 MB/s\. With such a limited Amazon FSx capacity, scaling your distributed training job up to larger clusters might aggravate I/O bottleneck problems; model training throughput might improve in later epochs as cache builds up, but Amazon FSx throughput can still be a bottleneck\.

To alleviate such I/O bottleneck problems, you should increase the Amazon FSx storage size to obtain a higher throughput capacity\. Typically, to find an optimal I/O throughput, you may experiment with different Amazon FSx throughput capacities, assigning an equal to or slightly lower throughput than your estimate, until you find that it is sufficient to resolve the I/O bottleneck problems\. In case of the aforementioned example, Amazon FSx storage with 2\.4 GB/s throughput and 67 GB RAM cache would be sufficient\. If the file system has an optimal throughput, the model training throughput should reach maximum either immediately or after the first epoch as cache has built up\.

To learn more about how to increase Amazon FSx storage and deployment types, see the following pages in the *Amazon FSx for Lustre documentation*:
+  [How to increase storage capacity](https://docs.aws.amazon.com/fsx/latest/LustreGuide/managing-storage-capacity.html#increase-storage-capacity) 
+  [Aggregate file system performance](https://docs.aws.amazon.com/fsx/latest/LustreGuide/performance.html#fsx-aggregate-perf) 