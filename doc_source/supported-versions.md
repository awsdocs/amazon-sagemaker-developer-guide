# Supported Versions<a name="supported-versions"></a>

Amazon SageMaker supports the following versions of learning frameworks and computing systems\.

**Deep Learning Framework Containers**

Amazon SageMaker Deep Learning containers support:

+ TensorFlow 1\.4 and 1\.5

+ Apache MXNet 0\.12 and 1\.0

Containers use the most recent version of a framework by default\. To use an earlier version, set the value of the `framework_version` parameter in the TensorFlow and MXNet estimators\. For more information about using TensorFlow and MXNet estimators, see the [Amazon SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk)\.

When using your own algorithms, you can use any version of the frameworks in your Docker image\.

**Notebook Instances**

Amazon SageMaker notebook instances are installed with kernels that support:

+ TensorFlow 1\.4

+ Apache MXNet 0\.12

+ Apache Spark 2\.1\.1 and 2\.2\.0