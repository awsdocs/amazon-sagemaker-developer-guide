# Bring your own SageMaker image<a name="studio-byoi"></a>

A SageMaker image is a file that identifies the kernels, language packages, and other dependencies required to run a Jupyter notebook in Amazon SageMaker Studio\. These images are used to create an environment that you then run the Jupyter notebooks from\. Amazon SageMaker provides many built\-in images for you to use\. If you need different functionality, you can bring your own custom images to Studio\. For the list of built\-in images, see [Available Amazon SageMaker Images](notebooks-available-images.md)\.

A SageMaker *image* is a holder for a set of SageMaker *image versions*\. An image version represents a container image that is compatible with SageMaker Studio and stored in an Amazon Elastic Container Registry \(ECR\) repository\. Each image version is immutable\.

To make a custom SageMaker image available to all users within a domain, you attach the image to the domain\. To make an image available to a single user, you attach the image to the user's profile\. When you attach an image, SageMaker uses the latest image version by default\. You can also attach a specific image version\. After you attach the version, you can choose the version from the SageMaker Launcher or the image selector when you launch a notebook\.

You can create images and image versions, and attach image versions to your domain, using the SageMaker Studio control panel, the [AWS SDK for Python \(Boto3\)](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html), and the [AWS Command Line Interface \(AWS CLI\)](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/)\. You can also create images and image versions using the SageMaker console, even if you haven't onboarded to Studio\.

SageMaker provides sample Dockerfiles to use as a starting point for your custom SageMaker images in the [SageMaker Studio Custom Image Samples](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/) repository\. These include Dockerfiles for the following images:
+ Julia
+ R
+ Scala
+ Tensorflow 2

The following topics explain how to bring your own image using the SageMaker console and then launch the image in SageMaker Studio\. A more comprehensive tutorial additionally shows you how to build a custom container image from a supplied R Dockerfile\. For a similar blog article, see [Bringing your own R environment to Amazon SageMaker Studio](http://aws.amazon.com/blogs/machine-learning/bringing-your-own-r-environment-to-amazon-sagemaker-studio/)\.

**Topics**
+ [Create a custom SageMaker image \(Console\)](studio-byoi-create.md)
+ [Attach a custom SageMaker image \(Control Panel\)](studio-byoi-attach.md)
+ [Launch a custom SageMaker image in SageMaker Studio](studio-byoi-launch.md)
+ [Bring your own custom SageMaker image tutorial](studio-byoi-create-sdk.md)
+ [Custom SageMaker image specifications](studio-byoi-specs.md)