# Edge Devices<a name="neo-edge-devices"></a>

Amazon SageMaker Neo provides compilation support for popular machine learning frameworks\. You can deploy your Neo\-compiled edge devices such as the Raspberry Pi 3, Texas Instruments' Sitara, Jetson TX1, and more\. For a full list of supported frameworks and edge devices, see [Supported Frameworks, Devices, Systems, and Architectures](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-supported-devices-edge.html)\. 

You must configure your edge device so that it can use AWS services\. One way to do this is to install DLR and Boto3 to your device\. To do this, youmust set up the authentication credentials\. See [Boto3 AWS Configuration](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html#configuration) for more information\. Once your model is compiled and your edge device is configured, you can download the model from Amazon S3 to your edge device\. From there, you can use the [Deep Learning Runtime \(DLR\)](https://neo-ai-dlr.readthedocs.io/en/latest/index.html) to read the compiled model and make inferences\. 

For first\-time users, we recommend you check out the [Getting Started](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-getting-started-edge.html) guide\. This guide walks you through how to set up your credentials, compile a model, deploy your model to a Raspberry Pi 3, and make inferences on images\. 

**Topics**
+ [Supported Frameworks, Devices, Systems, and Architectures](neo-supported-devices-edge.md)
+ [Deploy Models](neo-deployment-edge.md)
+ [Getting Started with Neo on Edge Devices](neo-getting-started-edge.md)