# Add a Studio\-compatible container image to Amazon ECR<a name="studio-byoi-sdk-add-container-image"></a>

You perform the following steps to add a container image to Amazon ECR:
+ Create an Amazon ECR repository\.
+ Authenticate to Amazon ECR\.
+ Build a Studio\-compatible container image\.
+ Push the image to the Amazon ECR repository\.

**Note**  
The Amazon ECR repository must be in the same AWS Region as SageMaker Studio\.

**To build and add a container image to Amazon ECR**

1. Create an Amazon ECR repository using the AWS CLI\. To create the repository using the Amazon ECR console, see [Creating a repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html)\.

   ```
   aws ecr create-repository \
       --repository-name smstudio-custom \
       --image-scanning-configuration scanOnPush=true
   ```

   Response:

   ```
   {
       "repository": {
           "repositoryArn": "arn:aws:ecr:us-east-2:acct-id:repository/smstudio-custom",
           "registryId": "acct-id",
           "repositoryName": "smstudio-custom",
           "repositoryUri": "acct-id.dkr.ecr.us-east-2.amazonaws.com/smstudio-custom",
           ...
       }
   }
   ```

1. Install the SageMaker Studio image build CLI by following the steps in [SageMaker Docker Build](https://github.com/aws-samples/sagemaker-studio-image-build-cli)\. This CLI enables you to build a Dockerfile using AWS CodeBuild\.

1. Build the R image `Dockerfile` using the Studio image build CLI\. The period \(\.\) specifies that the Dockerfile should be in the context of the build command\. This command builds the image, creates an ECR repo, and uploads the built image to the ECR repo\. It then outputs the image URI\.

   ```
   sm-docker build . -t smstudio-r -t <acct-id>.dkr.ecr.<region>.amazonaws.com/smstudio-custom:r
   ```

   Response:

   ```
   Image URI: <acct-id>.dkr.ecr.<region>.amazonaws.com/<image_name>
   ```