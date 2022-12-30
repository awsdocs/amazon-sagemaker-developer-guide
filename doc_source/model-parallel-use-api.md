# Run a SageMaker Distributed Training Job with Model Parallelism<a name="model-parallel-use-api"></a>

Learn how to run a model parallel training job using the SageMaker Python SDK with your own training script and SageMaker's model parallelism library\.

There are three use\-case scenarios for running a SageMaker training job:

1. You can use one of the prebuilt AWS Deep Learning Container for TensorFlow and PyTorch\. This option is recommended if it is the first time for you to use the model parallel library\. To find a tutorial for how to run a SageMaker model parallel training job, see the example notebooks at [PyTorch training with Amazon SageMaker's model parallelism library](https://github.com/aws/amazon-sagemaker-examples/tree/main/training/distributed_training/pytorch/model_parallel)\.

1. You can extend the prebuilt containers to handle any additional functional requirements for your algorithm or model that the prebuilt SageMaker Docker image doesn't support\. To find an example of how you can extend a pre\-built container, see [Extend a Prebuilt Container](https://docs.aws.amazon.com/sagemaker/latest/dg/prebuilt-containers-extend.html)\.

1. You can adapt your own Docker container to work with SageMaker using the [SageMaker Training toolkit](https://github.com/aws/sagemaker-training-toolkit)\. For an example, see [Adapting Your Own Training Container](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html)\.

For options 2 and 3 in the preceding list, refer to [Extend a Prebuilt Docker Container that Contains SageMaker's Distributed Model Parallel Library](model-parallel-sm-sdk.md#model-parallel-customize-container) to learn how to install the model parallel library in an extended or customized Docker container\. 

In all cases, you launch your training job configuring a SageMaker `TensorFlow` or `PyTorch` estimator to initialize the library\. To learn more, see the following topics\.

**Topics**
+ [Step 1: Modify Your Own Training Script Using SageMaker's Distributed Model Parallel Library](model-parallel-customize-training-script.md)
+ [Step 2: Launch a Training Job Using the SageMaker Python SDK](model-parallel-sm-sdk.md)