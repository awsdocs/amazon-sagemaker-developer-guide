# Modify Your Training Script Using the SageMaker Data Parallel Library<a name="data-parallel-modify-sdp"></a>

## Script Modification Overview<a name="data-parallel-modify-sdp-overview"></a>

SageMaker's distributed data parallel library \(the library\) APIs are designed for ease of use, and to provide seamless integration with existing distributed training toolkits\.
+ *SageMaker Python SDK with the library API*: In most cases, all you have to change in your training script is the Horovod or other data parallel library import statements\. Swap these out with the SageMaker data parallel library equivalents\.
+ *Focus on your model training without infrastructure management*: When training a deep learning model with the library on SageMaker, you can focus on your model training, while SageMaker does cluster management: brings up the nodes and creates the cluster, completes the training, then tears down the cluster\. 

 To customize your own training script, you need to do the following: 
+ You must provide TensorFlow/PyTorch training scripts that are adapted to use the library\. The following sections provide example code for this\. 
+ Your input data must be in an S3 bucket or in [FSx](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html) in the AWS Region that you will use to launch your training job\. If you use the Jupyter Notebooks provided, create a SageMaker notebook instance in the same Region as the bucket that contains your input data\. For more information about storing your training data, refer to the [SageMaker Python SDK data inputs](https://sagemaker.readthedocs.io/en/stable/overview.html#use-file-systems-as-training-input) documenation\. 

**Tip**  
Consider using FSx instead of Amazon S3 to increase training performance\. It has higher throughput and lower latency than Amazon S3\. 

Use the following sections to see examples of adapting the library to your TensorFlow or PyTorch training scripts\. Once you have launched a training job, you can monitor system utilization and model performance using [Amazon SageMaker Debugger](train-debugger.md) or Amazon CloudWatch\.

**Topics**
+ [Script Modification Overview](#data-parallel-modify-sdp-overview)
+ [Modify a TensorFlow Training Script](data-parallel-modify-sdp-tf2.md)
+ [Modify a PyTorch Training Script](data-parallel-modify-sdp-pt.md)