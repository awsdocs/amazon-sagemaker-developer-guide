# Supported Frameworks and Algorithms<a name="debugger-supported-frameworks"></a>

The following table shows SageMaker machine learning frameworks and algorithms supported by Debugger\. 


| 
| 
| SageMaker frameworks and algorithms |  Monitoring system bottlenecks  |  Profiling deep learning framework operations  |  Debugging model parameters  | 
| --- |--- |--- |--- |
|  [TensorFlow](https://sagemaker.readthedocs.io/en/stable/using_tf.html)   |  All [AWS Deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers)  |  [AWS TensorFlow deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) 2\.3\.1 or later  |  [AWS TensorFlow deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) 1\.15\.4 or later  | 
|  [PyTorch](https://sagemaker.readthedocs.io/en/stable/using_pytorch.html)  |  [AWS PyTorch deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) 1\.6\.0 or later  |  [AWS PyTorch deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) 1\.5\.0 or later  | 
|  [MXNet](https://sagemaker.readthedocs.io/en/stable/using_mxnet.html)   |  \-  |  [AWS MXNet deep learning containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#general-framework-containers) 1\.6\.0 or later  | 
|  [XGBoost](https://sagemaker.readthedocs.io/en/stable/frameworks/xgboost/using_xgboost.html)  |  1\.0\-1, 1\.2\-1  | \- |  1\.0\-1, 1\.2\-1  | 
|  [SageMaker generic estimator](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html)  |  SageMaker built\-in algorithms using image URIs [Custom training containers](debugger-bring-your-own-container.md) \(with the AWS deep learning container images, public Docker images, or your own Docker images\)  | \- |  [Custom training containers](debugger-bring-your-own-container.md) \(available for TensorFlow, PyTorch, MXNet, and XGBoost with manual hook registration\)  | 
+ **Monitoring system bottlenecks** – Monitor the system utilization rate for resources such as CPU, GPU, memories, network, and data I/O metrics\. This is a framework and model agnostic feature and available for any training jobs in SageMaker\.
+ **Profiling deep learning framework operations** – Profile the deep learning operations of the TensorFlow and PyTorch frameworks, such as step durations, data loaders, forward and backward operations, Python profiling metrics, and framework\-specific metrics\.
+ **Debugging model parameters** – Track and debug model parameters, such as weights, gradients, biases, and scalar values of your training job\. Available deep learning frameworks are Apache MXNet, TensorFlow, PyTorch, and XGBoost\.

If the framework or algorithm that you want to train and debug is not listed in the table, go to the [AWS Discussion Forum](https://forums.aws.amazon.com/) and leave feedback on SageMaker Debugger\.

## Use Debugger with Custom Training Containers<a name="debugger-byoc-intro"></a>

Bring your training containers to SageMaker and gain insights into your training jobs using Debugger\. Maximize your work efficiency by optimizing your model on Amazon EC2 instances using the monitoring and debugging features\.

For more information about how to build your training container with the `SMDebug` client library, push it to the Amazon Elastic Container Registry \(Amazon ECR\), and monitor and debug, see [Use Debugger with Custom Training Containers](debugger-bring-your-own-container.md)\.

## Debugger Open\-Source GitHub Repositories<a name="debugger-opensource"></a>

Debugger APIs are provided through the SageMaker Python SDK and designed to construct Debugger hook and rule configurations for the SageMaker [ CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) and [ DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html) API operations\. The `SMDebug` client library provides tools to register *hooks* and access the training data through its *trial* feature, all through its flexible and powerful API operations\. It supports the machine learning frameworks TensorFlow, PyTorch, MXNet, and XGBoost on Python 3\.6 and later\. 

For direct resources about the Debugger and `SMDebug` API operations, see the following links: 
+ [The Amazon SageMaker Python SDK documentation](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_debugger.html)
+ [The Amazon SageMaker Debugger APIs](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html)
+ [The SMDebug open source client library](https://github.com/awslabs/sagemaker-debugger#amazon-sagemaker-debugger)
+ [The SMDebug PyPI](https://pypi.org/project/smdebug/)

If you use the SDK for Java to conduct SageMaker training jobs and want to configure Debugger APIs, see the following references:
+ [ Amazon SageMaker Debugger API Operations](debugger-apis.md)
+ [Configure Debugger Using Amazon SageMaker API](debugger-createtrainingjob-api.md)