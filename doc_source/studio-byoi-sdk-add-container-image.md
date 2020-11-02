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

1. Authenticate to Amazon ECR\. Make sure the Docker application is running\. For more information, see [Registry Authentication](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth)\.
**Note**  
The `get-login-password` command was introduced in AWS CLI version 1\.17\.10\. Use `aws --version` to determine your version of the AWS CLI\. For information about upgrading, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)\.

   ```
   aws ecr get-login-password | \
       docker login --username AWS --password-stdin <acct-id>.dkr.ecr.<region>.amazonaws.com/smstudio-custom
   ```

   Response:

   ```
   Login Succeeded
   ```

1. Build the R image `Dockerfile`\. The period \(\.\) specifies that the Dockerfile should be in the context of the build command\.
**Note**  
You can't directly build a Dockerfile within Studio\.

   ```
   docker build . -t smstudio-r -t <acct-id>.dkr.ecr.<region>.amazonaws.com/smstudio-custom:r
   ```

   Response:

   ```
   Successfully built f97aaaa805b1
   Successfully tagged smstudio-r:latest
   Successfully tagged acct-id.dkr.ecr.us-east-2.amazonaws.com/smstudio-custom:r
   ```

1. Push the container image to the Amazon ECR repository\. For more information, see [ImagePush](https://docs.docker.com/engine/api/v1.40/#operation/ImagePush) and [Pushing an image](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html)\.

   ```
   docker push <acct-id>.dkr.ecr.<region>.amazonaws.com/smstudio-custom:r
   ```

   Response:

   ```
   The push refers to repository [acct-id.dkr.ecr.us-east-2.amazonaws.com/smstudio-custom]
   r: digest: sha256:7a5c8cb01944f3b10fe495a930b3682d15b7b1de9f1f5497d1a3e3634369cead size: 3066
   ```