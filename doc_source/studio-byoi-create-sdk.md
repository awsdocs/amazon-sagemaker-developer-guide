# Bring your own custom SageMaker image tutorial<a name="studio-byoi-create-sdk"></a>

In this tutorial, you create a custom SageMaker image and attach a version of the image to your domain for use in Amazon SageMaker Studio\. The image version contains a selection of R packages, along with the AWS SDK for Python \(Boto3\) and the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. After you complete this tutorial, you can select the version in Studio and use R to access the SDKs using the RStudio reticulate package\. For more information on the reticulate package, see [R Interface to Python](https://github.com/rstudio/reticulate)\. For a blog article similar to this tutorial, see [Bringing your own R environment to Amazon SageMaker Studio](http://aws.amazon.com/blogs/machine-learning/bringing-your-own-r-environment-to-amazon-sagemaker-studio/)\.

Two methods are presented to attach the image version to your domain\. In the first method, you create a new domain with the version attached\. This method is simpler but you need to specify the Amazon Virtual Private Cloud \(VPC\) information and execution role that's required to create the domain\.

If you have onboarded to Studio, you can use the second method to attach the image version to your current domain\. In this case, you don't need to specify the VPC information and execution role\. After you attach the version, you must delete all the apps in your domain and reopen Studio\.

You can't run this tutorial from Studio for the following reasons:
+ Docker isn't available inside Studio\.
+ You can't create or update a domain within Studio\.

**Prerequisites**
+ The Docker application\. For information about setting up Docker, see [Orientation and setup](https://docs.docker.com/get-started/)\.
+ A local copy of the Dockerfile for creating a Studio compatible [R image](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/tree/main/examples/r-image) from the [SageMaker Studio custom image samples](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/) repository\.
**Note**  
Building the R image from the Dockerfile installs dependencies that may be licensed under copyleft licenses such as GPLv3\. You should review the license terms and make sure they are acceptable for your use case before proceeding and building this image\.
+ Permissions to access the Amazon Elastic Container Registry \(Amazon ECR\) service\. For more information, see [Amazon ECR Managed Policies](https://docs.aws.amazon.com/AmazonECR/latest/userguide/ecr_managed_policies.html)\.
+ An AWS Identity and Access Management execution role that has the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\. If you have onboarded to Amazon SageMaker Studio, you can get the role from the **Studio Summary** section of the SageMaker Studio control panel\.

**Links**
+ [Amazon Elastic Container Registry API Reference](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/)
+ [Docker Engine API](https://docs.docker.com/engine/api/v1.40/)
+ [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

**Topics**
+ [Add a Studio\-compatible container image to Amazon ECR](studio-byoi-sdk-add-container-image.md)
+ [Create a SageMaker image from the ECR container image](studio-byoi-sdk-create-image.md)
+ [Attach the SageMaker image to a new domain](studio-byoi-sdk-attach-new-domain.md)
+ [Attach the SageMaker image to your current domain](studio-byoi-sdk-attach-current-domain.md)
+ [View the attached image in the Studio control panel](studio-byoi-sdk-view.md)
+ [Clean up resources](studio-byoi-sdk-cleanup.md)