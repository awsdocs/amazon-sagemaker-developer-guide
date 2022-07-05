# Bring your own image to RStudio on SageMaker<a name="rstudio-byoi"></a>

A SageMaker image is a file that identifies language packages and other dependencies that are required to run RStudio on Amazon SageMaker\. SageMaker uses these images to create an environment where you run RStudio\. Amazon SageMaker provides a built\-in RStudio image for you to use\. If you need different functionality, you can bring your own custom images\.

The process to bring your own image to use with RStudio on SageMaker takes three steps:

1. Build a custom image from a Dockerfile and push it to a repository in Amazon Elastic Container Registry \(Amazon ECR\)\.

1. Create a SageMaker image that points to a container image in Amazon ECR and attach it to your SageMaker domain\.

1. Launch a new session in RStudio with your custom image\.

You can create images and image versions, and attach image versions to your domain, using the SageMaker control panel, the [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html), and the [AWS Command Line Interface \(AWS CLI\)](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/)\. You can also create images and image versions using the SageMaker console, even if you haven't onboarded to a domain\.

The following topics show how to bring your own image to RStudio on SageMaker by creating, attaching, and launching a custom image\.

## Key terminology<a name="rstudio-byoi-basics"></a>

The following section defines key terms for bringing your own image to use with RStudio on SageMaker\.
+ **Dockerfile:** A Dockerfile is a file that identifies the language packages and other dependencies for your Docker image\.
+ **Docker image:** The Docker image is a built Dockerfile\. This image is checked into Amazon ECR and serves as the basis of the SageMaker image\.
+ **SageMaker image:** A SageMaker image is a holder for a set of SageMaker image versions based on Docker images\. 
+ **Image version:** An image version of a SageMaker image represents a Docker image that is compatible with RStudio and stored in an Amazon ECR repository\. Each image version is immutable\. These image versions can be attached to a domain and used with RStudio on SageMaker\.