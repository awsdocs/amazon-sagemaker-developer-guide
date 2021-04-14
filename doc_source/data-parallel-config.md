# SageMaker distributed data parallel Configuration Tips and Pitfalls<a name="data-parallel-config"></a>

Review the following tips and pitfalls before using SageMaker's distributed data parallel library\. This list includes tips that are applicable across frameworks\.

**Topics**
+ [Data Preprocessing](#data-parallel-config-dataprep)
+ [Single vs Multiple Nodes](#data-parallel-config-multi-node)
+ [Debug Scaling Efficiency with Debugger](#data-parallel-config-debug)
+ [Batch Size](#data-parallel-config-batch-size)

## Data Preprocessing<a name="data-parallel-config-dataprep"></a>

If you pre\-process data during training using an external library that utilizes the CPU, you may run into a CPU bottleneck because SageMaker distributed data parallel uses the CPU for all\-reduce operations\. You may be able to improve training time by moving pre\-processing steps to a library that uses GPUs or by completing all pre\-processing before training\.

## Single vs Multiple Nodes<a name="data-parallel-config-multi-node"></a>

It is recommended that you use this library with multiple nodes\. The library can be used with a single\-host, multi\-device setup \(for example, a single ML compute instance with multiple GPUs\), however, when you use two or more nodes, the library’s AllReduce operation gives you significant performance improvement\. Also, on a single host, NVLink already contributes to in\-node AllReduce efficiency\.

## Debug Scaling Efficiency with Debugger<a name="data-parallel-config-debug"></a>

You can use Amazon SageMaker Debugger to monitor and visualize CPU and GPU utilization, and other metrics of interest, during training\. You can use Debugger [built\-in rules](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) to monitor computational performance issues, such as CPUBottleneck, LoadBalancing, and LowGPUUtilization\. You can specify these rules with [Debugger configurations](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-configuration.html) when you define an Amazon SageMaker Python SDK estimator\. If you use AWS CLI and AWS Boto3 for training on SageMaker, you can enable Debugger as shown in [Configure Debugger Using Amazon SageMaker API](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-createtrainingjob-api.html)\.

To see an example using Debugger in a SageMaker training job, you can reference one of the notebook example in the [ SageMaker Notebook Examples GitHub repository](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger)\. To learn more about Debugger, see [Amazon SageMaker Debugger](https://docs.aws.amazon.com/sagemaker/latest/dg/train-debugger.html)\.

## Batch Size<a name="data-parallel-config-batch-size"></a>

In distributed training, as more nodes are added, batch sizes should increase proportionally\. To improve convergence speed as you add more nodes to your training job and increase the global batch size, increase the learning rate\.

One way to achieve this is by using a gradual learning rate warmup where the learning rate is ramped up from a small to a large value as the training job progresses\. This ramp avoids a sudden increase of the learning rate, allowing healthy convergence at the start of training\. For example, you can use a “Linear Scaling Rule” where each time the mini\-batch size is multiplied by k, the learning rate is also multiplied by k\. To learn more about this technique, see the research paper, Accurate, [ Large Minibatch SGD: Training ImageNet in 1 Hour](https://arxiv.org/pdf/1706.02677.pdf), Sections 2 and 3\.