# Prerequisites<a name="studio-byoi-prereq"></a>

You must satisfy the following prerequisites to bring your own container for use with Amazon SageMaker Studio\.
+ The Docker application\. For information about setting up Docker, see [Orientation and setup](https://docs.docker.com/get-started/)\.
+ Install the AWS CLI by following the steps in [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.
+ A local copy of any Dockerfile for creating a Studio compatible image\. For sample custom images, see the [SageMaker Studio custom image samples](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/) repository\.
+ Permissions to access the Amazon Elastic Container Registry \(Amazon ECR\) service\. For more information, see [Amazon ECR Managed Policies](https://docs.aws.amazon.com/AmazonECR/latest/userguide/ecr_managed_policies.html)\.
+ An AWS Identity and Access Management execution role that has the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\. If you have onboarded to Amazon SageMaker domain, you can get the role from the **Domain Summary** section of the SageMaker control panel\.
+ Install the Studio image build CLI by following the steps in [SageMaker Docker Build](https://github.com/aws-samples/sagemaker-studio-image-build-cli)\. This CLI enables you to build a Dockerfile using AWS CodeBuild\.