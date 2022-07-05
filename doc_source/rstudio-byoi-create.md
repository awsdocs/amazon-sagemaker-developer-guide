# Create a custom RStudio image<a name="rstudio-byoi-create"></a>

This topic describes how you can create a custom RStudio image using the SageMaker console and the AWS CLI\. If you use the AWS CLI, you must run the steps from your local machine\. The following steps do not work from within Amazon SageMaker Studio\.

When you create an image, SageMaker also creates an initial image version\. The image version represents a container image in [Amazon Elastic Container Registry \(ECR\)](https://console.aws.amazon.com/ecr/)\. The container image must satisfy the requirements to be used in RStudio\. For more information, see [Custom RStudio image specifications](rstudio-byoi-specs.md)\.

For information about testing your image locally and resolving common issues, see the [SageMaker Studio Custom Image Samples repo](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/blob/main/DEVELOPMENT.md)\.

**Topics**
+ [Add a SageMaker\-compatible RStudio Docker container image to Amazon ECR](#rstudio-byoi-sdk-add-container-image)
+ [Create a SageMaker image from the console](#rstudio-byoi-create-console)
+ [Create an image from the AWS CLI](#rstudio-byoi-create-cli)

## Add a SageMaker\-compatible RStudio Docker container image to Amazon ECR<a name="rstudio-byoi-sdk-add-container-image"></a>

Use the following steps to add a Docker container image to Amazon ECR:
+ Create an Amazon ECR repository\.
+ Authenticate to Amazon ECR\.
+ Build a SageMaker\-compatible RStudio Docker image\.
+ Push the image to the Amazon ECR repository\.

**Note**  
The Amazon ECR repository must be in the same AWS Region as your domain\.

**To build and add a Docker image to Amazon ECR**

1. Create an Amazon ECR repository using the AWS CLI\. To create the repository using the Amazon ECR console, see [Creating a repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html)\.

   ```
   aws ecr create-repository \
       --repository-name rstudio-custom \
       --image-scanning-configuration scanOnPush=true
   ```

   Response:

   ```
   {
       "repository": {
           "repositoryArn": "arn:aws:ecr:us-east-2:acct-id:repository/rstudio-custom",
           "registryId": "acct-id",
           "repositoryName": "rstudio-custom",
           "repositoryUri": "acct-id.dkr.ecr.us-east-2.amazonaws.com/rstudio-custom",
           ...
       }
   }
   ```

1. Authenticate to Amazon ECR using the repository URI returned as a response from the `create-repository` command\. Make sure that the Docker application is running\. For more information, see [Registry Authentication](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth)\.

   ```
   aws ecr get-login-password | \
       docker login --username AWS --password-stdin <repository-uri>
   ```

   Response:

   ```
   Login Succeeded
   ```

1. Build the Docker image\. Run the following command from the directory that includes your Dockerfile\.

   ```
   docker build .
   ```

1. Tag your built image with a unique tag\.

   ```
   docker tag <image-id> "<repository-uri>:<tag>"
   ```

1. Push the container image to the Amazon ECR repository\. For more information, see [ImagePush](https://docs.docker.com/engine/api/v1.40/#operation/ImagePush) and [Pushing an image](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html)\.

   ```
   docker push <repository-uri>:<tag>
   ```

   Response:

   ```
   The push refers to repository [<account-id>.dkr.ecr.us-east-2.amazonaws.com/rstudio-custom]
   r: digest: <digest> size: 3066
   ```

## Create a SageMaker image from the console<a name="rstudio-byoi-create-console"></a>

**To create an image**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Images**\.

1. On the **Custom images** page, choose **Create image**\.

1. For **Image source**, enter the registry path to the container image in Amazon ECR\. The path is in the following format:

   ` acct-id.dkr.ecr.region.amazonaws.com/repo-name[:tag] or [@digest] `

1. Choose **Next**\.

1. Under **Image properties**, enter the following:
   + Image name – The name must be unique to your account in the current AWS Region\.
   + \(Optional\) Image display name – The name displayed in the domain user interface\. When not provided, `Image name` is displayed\.
   + \(Optional\) Description – A description of the image\.
   + IAM role – The role must have the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\. Use the dropdown menu to choose one of the following options:
     + Create a new role – Specify any additional Amazon Simple Storage Service \(Amazon S3\) buckets that you want your notebooks users to access\. If you don't want to allow access to additional buckets, choose **None**\.

       SageMaker attaches the `AmazonSageMakerFullAccess` policy to the role\. The role allows your notebook users to access the Amazon S3 buckets listed next to the check marks\.
     + Enter a custom IAM role ARN – Enter the Amazon Resource Name \(ARN\) of your IAM role\.
     + Use existing role – Choose one of your existing roles from the list\.
   + \(Optional\) Image tags – Choose **Add new tag**\. You can add up to 50 tags\. Tags are searchable using the SageMaker console or the SageMaker `Search` API\.

1. Under **Image type**, select RStudio image\.

1. Choose **Submit**\.

The new image is displayed in the **Custom images** list and briefly highlighted\. After the image has been successfully created, you can choose the image name to view its properties or choose **Create version** to create another version\.

**To create another image version**

1. Choose **Create version** on the same row as the image\.

1. For **Image source**, enter the registry path to the Amazon ECR image\. The image shouldn't be the same image as used in a previous version of the SageMaker image\.

To use the custom image in RStudio, you must attach it to your domain\. For more information, see [Attach a custom SageMaker image](rstudio-byoi-attach.md)\.

## Create an image from the AWS CLI<a name="rstudio-byoi-create-cli"></a>

This section shows how to create a custom Amazon SageMaker image using the AWS CLI\.

Use the following steps to create a SageMaker image:
+ Create an `Image`\.
+ Create an `ImageVersion`\.
+ Create a configuration file\.
+ Create an `AppImageConfig`\.

**To create the SageMaker image entities**

1. Create a SageMaker image\. The role ARN must have at least the `AmazonSageMakerFullAccessPolicy` policy attached\.

   ```
   aws sagemaker create-image \
       --image-name rstudio-custom-image \
       --role-arn arn:aws:iam::<acct-id>:role/service-role/<execution-role>
   ```

   Response:

   ```
   {
       "ImageArn": "arn:aws:sagemaker:us-east-2:acct-id:image/rstudio-custom-image"
   }
   ```

1. Create a SageMaker image version from the image\. Pass the unique tag value that you chose when you pushed the image to Amazon ECR\.

   ```
   aws sagemaker create-image-version \
       --image-name rstudio-custom-image \
       --base-image <repository-uri>:<tag>
   ```

   Response:

   ```
   {
       "ImageVersionArn": "arn:aws:sagemaker:us-east-2:acct-id:image-version/rstudio-image/1"
   }
   ```

1. Check that the image version was successfully created\.

   ```
   aws sagemaker describe-image-version \
       --image-name rstudio-custom-image \
       --version 1
   ```

   Response:

   ```
   {
       "ImageVersionArn": "arn:aws:sagemaker:us-east-2:acct-id:image-version/rstudio-custom-image/1",
       "ImageVersionStatus": "CREATED"
   }
   ```
**Note**  
If the response is `"ImageVersionStatus": "CREATED_FAILED"`, the response also includes the failure reason\. A permissions issue is a common cause of failure\. You also can check your Amazon CloudWatch Logs\. The name of the log group is `/aws/sagemaker/studio`\. The name of the log stream is `$domainID/$userProfileName/KernelGateway/$appName`\.

1. Create a configuration file, named `app-image-config-input.json`\. The app image config is used to configuration for running a SageMaker image as a Kernel Gateway application\.

   ```
   {
       "AppImageConfigName": "rstudio-custom-config"
   }
   ```

1. Create the AppImageConfig using the file that you created in the previous step\.

   ```
   aws sagemaker create-app-image-config \
       --cli-input-json file://app-image-config-input.json
   ```

   Response:

   ```
   {
       "AppImageConfigArn": "arn:aws:sagemaker:us-east-2:acct-id:app-image-config/r-image-config"
   }
   ```