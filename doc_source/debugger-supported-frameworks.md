# Supported Frameworks and Algorithms<a name="debugger-supported-frameworks"></a>

The following table shows SageMaker machine learning frameworks and algorithms supported by Debugger\. 


| 
| 
| SageMaker\-supported frameworks and algorithms |  Monitoring system bottlenecks  |  Profiling deep learning framework operations  |  Debugging output tensors  | 
| --- |--- |--- |--- |
|  [TensorFlow](https://sagemaker.readthedocs.io/en/stable/using_tf.html)   |  All [AWS Deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers)  |  [AWS TensorFlow deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) >= v2\.3\.1, < v2\.11  |  [AWS TensorFlow deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) 1\.15\.4 or later  | 
|  [PyTorch](https://sagemaker.readthedocs.io/en/stable/using_pytorch.html)  |  [AWS PyTorch deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) >= v1\.6\.0, < v2\.0  |  [AWS PyTorch deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) 1\.5\.0 or later  | 
|  [MXNet](https://sagemaker.readthedocs.io/en/stable/using_mxnet.html)   |  \-  |  [AWS MXNet deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) 1\.6\.0 or later  | 
|  [XGBoost](https://sagemaker.readthedocs.io/en/stable/frameworks/xgboost/using_xgboost.html)  |  1\.0\-1, 1\.2\-1, 1\.3\-1  | \- |  1\.0\-1, 1\.2\-1, 1\.3\-1  | 
|  [SageMaker generic estimator](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html)  |  SageMaker built\-in algorithms using image URIs [Custom training containers](debugger-bring-your-own-container.md) \(with the AWS deep learning container images, public Docker images, or your own Docker images\)  | \- |  [Custom training containers](debugger-bring-your-own-container.md) \(available for TensorFlow, PyTorch, MXNet, and XGBoost with manual hook registration\)  | 
+ **Monitoring system bottlenecks** – Monitor the system utilization rate for resources such as CPU, GPU, memories, network, and data I/O metrics\. This is a framework and model agnostic feature and available for any training jobs in SageMaker\.
+ **Profiling deep learning framework operations** – Profile the deep learning operations of the TensorFlow and PyTorch frameworks, such as step durations, data loaders, forward and backward operations, Python profiling metrics, and framework\-specific metrics\.
**Warning**  
SageMaker Debugger deprecates the framework profiling feature starting from TensorFlow 2\.11 and PyTorch 2\.0\. You can still use the feature in the previous versions of the frameworks and SDKs as follows\.   
SageMaker Python SDK <= v2\.130\.0
PyTorch >= v1\.6\.0, < v2\.0
TensorFlow >= v2\.3\.1, < v2\.11
See also [Amazon SageMaker Debugger Release Notes: March 16, 2023](debugger-release-notes.md#debugger-release-notes-20230315)\.
+ **Debugging output tensors** – Track and debug model parameters, such as weights, gradients, biases, and scalar values of your training job\. Available deep learning frameworks are Apache MXNet, TensorFlow, PyTorch, and XGBoost\.
**Important**  
 For the TensorFlow framework with Keras, SageMaker Debugger deprecates the zero code change support for debugging models built using the `tf.keras` modules of TensorFlow 2\.6 and later\. This is due to breaking changes announced in the [TensorFlow 2\.6\.0 release note](https://github.com/tensorflow/tensorflow/releases/tag/v2.6.0)\. For instructions on how to update your training script, see [Adapt Your TensorFlow Training Script](debugger-modify-script-tensorflow.md)\.
**Important**  
Since PyTorch v1\.12\.0 and later, SageMaker Debugger deprecates the zero code change support for debugging models\.  
This is due to breaking changes that cause SageMaker Debugger to interfere with the `torch.jit` functionality\. For instructions on how to update your training script, see [Adapt Your PyTorch Training Script](debugger-modify-script-pytorch.md)\.

If the framework or algorithm that you want to train and debug is not listed in the table, go to the [AWS Discussion Forum](https://forums.aws.amazon.com/) and leave feedback on SageMaker Debugger\.

## AWS Regions<a name="debugger-support-aws-regions"></a>

Amazon SageMaker Debugger is available in all regions where Amazon SageMaker is in service except the following region\.
+ Asia Pacific \(Jakarta\): `ap-southeast-3`

To find if Amazon SageMaker is in service in your AWS Region, see [AWS Regional Services](http://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

## Use Debugger with Custom Training Containers<a name="debugger-byoc-intro"></a>

Bring your training containers to SageMaker and gain insights into your training jobs using Debugger\. Maximize your work efficiency by optimizing your model on Amazon EC2 instances using the monitoring and debugging features\.

For more information about how to build your training container with the `sagemaker-debugger` client library, push it to the Amazon Elastic Container Registry \(Amazon ECR\), and monitor and debug, see [Use Debugger with Custom Training Containers](debugger-bring-your-own-container.md)\.

## Debugger Open\-Source GitHub Repositories<a name="debugger-opensource"></a>

Debugger APIs are provided through the SageMaker Python SDK and designed to construct Debugger hook and rule configurations for the SageMaker [ CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) and [ DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html) API operations\. The `sagemaker-debugger` client library provides tools to register *hooks* and access the training data through its *trial* feature, all through its flexible and powerful API operations\. It supports the machine learning frameworks TensorFlow, PyTorch, MXNet, and XGBoost on Python 3\.6 and later\. 

For direct resources about the Debugger and `sagemaker-debugger` API operations, see the following links: 
+ [The Amazon SageMaker Python SDK documentation](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_debugger.html)
+ [The Amazon SageMaker Python SDK \- Debugger APIs](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html)
+ [The `sagemaker-debugger` Python SDK documentation](https://sagemaker-debugger.readthedocs.io/en/website/index.html) for [the Amazon SageMaker Debugger open source client library](https://github.com/awslabs/sagemaker-debugger#amazon-sagemaker-debugger)
+ [The `sagemaker-debugger` PyPI](https://pypi.org/project/smdebug/)

If you use the SDK for Java to conduct SageMaker training jobs and want to configure Debugger APIs, see the following references:
+ [ Amazon SageMaker Debugger API Operations](debugger-apis.md)
+ [Configure Debugger Using Amazon SageMaker API](debugger-createtrainingjob-api.md)