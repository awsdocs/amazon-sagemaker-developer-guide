# Bring your own SageMaker image<a name="studio-byoi"></a>

A SageMaker image is a file that identifies the kernels, language packages, and other dependencies required to run a Jupyter notebook in Amazon SageMaker Studio\. These images are used to create an environment that you then run Jupyter notebooks from\. Amazon SageMaker provides many built\-in images for you to use\. For the list of built\-in images, see [Available Amazon SageMaker Images](notebooks-available-images.md)\.

If you need different functionality, you can bring your own custom images to Studio\. You can create images and image versions, and attach image versions to your domain or shared space, using the SageMaker control panel, the [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html), and the [AWS Command Line Interface \(AWS CLI\)](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/)\. You can also create images and image versions using the SageMaker console, even if you haven't onboarded to a SageMaker domain\. SageMaker provides sample Dockerfiles to use as a starting point for your custom SageMaker images in the [SageMaker Studio Custom Image Samples](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/) repository\.

The following topics explain how to bring your own image using the SageMaker console or AWS CLI, then launch the image in Studio\. For a similar blog article, see [Bringing your own R environment to Amazon SageMaker Studio](http://aws.amazon.com/blogs/machine-learning/bringing-your-own-r-environment-to-amazon-sagemaker-studio/)\. For notebooks that show how to bring your own image for use in training and inference, see [Amazon SageMaker Studio Container Build CLI](https://github.com/aws/amazon-sagemaker-examples/tree/main/aws_sagemaker_studio/sagemaker_studio_image_build)\.

## Key terminology<a name="studio-byoi-basics"></a>

The following section defines key terms for bringing your own image to use with Studio\.
+ **Dockerfile:** A Dockerfile is a file that identifies the language packages and other dependencies for your Docker image\.
+ **Docker image:** The Docker image is a built Dockerfile\. This image is checked into Amazon ECR and serves as the basis of the SageMaker image\.
+ **SageMaker image:** A SageMaker image is a holder for a set of SageMaker image versions based on Docker images\. Each image version is immutable\.
+ **Image version:** An image version of a SageMaker image represents a Docker image and is stored in an Amazon ECR repository\. Each image version is immutable\. These image versions can be attached to a domain or shared space and used with Studio\.

**Topics**
+ [Key terminology](#studio-byoi-basics)
+ [Custom SageMaker image specifications](studio-byoi-specs.md)
+ [Prerequisites](studio-byoi-prereq.md)
+ [Add a Docker image compatible with Studio to Amazon ECR](studio-byoi-sdk-add-container-image.md)
+ [Create a custom SageMaker image](studio-byoi-create.md)
+ [Attach a custom SageMaker image](studio-byoi-attach.md)
+ [Launch a custom SageMaker image in Amazon SageMaker Studio](studio-byoi-launch.md)
+ [Clean up resources](studio-byoi-cleanup.md)