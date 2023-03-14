# Create a custom SageMaker image<a name="studio-byoi-create"></a>

This topic describes how you can create a custom SageMaker image using the SageMaker console or AWS CLI\.

When you create an image from the console, SageMaker also creates an initial image version\. The image version represents a container image in [Amazon Elastic Container Registry \(ECR\)](https://console.aws.amazon.com/ecr/)\. The container image must satisfy the requirements to be used in Amazon SageMaker Studio\. For more information, see [Custom SageMaker image specifications](studio-byoi-specs.md)\. For information on testing your image locally and resolving common issues, see the [SageMaker Studio Custom Image Samples repo](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/blob/main/DEVELOPMENT.md)\.

After you have created your custom SageMaker image, you must attach it to your domain or shared space to use it with Studio\. For more information, see [Attach a custom SageMaker image](studio-byoi-attach.md)\.

## Create a SageMaker image from the console<a name="studio-byoi-create-console"></a>

The following section demonstrates how to create a custom SageMaker image from the SageMaker console\.

**To create an image**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Images**\.

1. On the **Custom images** page, choose **Create image**\.

1. For **Image source**, enter the registry path to the container image in Amazon ECR\. The path is in the following format:

   ` acct-id.dkr.ecr.region.amazonaws.com/repo-name[:tag] or [@digest] `

1. Choose **Next**\.

1. Under **Image properties**, enter the following:
   + Image name – The name must be unique to your account in the current AWS Region\.
   + \(Optional\) Display name – The name displayed in the Studio user interface\. When not provided, `Image name` is displayed\.
   + \(Optional\) Description – A description of the image\.
   + IAM role – The role must have the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\. Use the dropdown menu to choose one of the following options:
     + Create a new role – Specify any additional Amazon Simple Storage Service \(Amazon S3\) buckets that you want users of your notebooks to have access to\. If you don't want to allow access to additional buckets, choose **None**\.

       SageMaker attaches the `AmazonSageMakerFullAccess` policy to the role\. The role allows users of your notebooks access to the S3 buckets listed next to the checkmarks\.
     + Enter a custom IAM role ARN – Enter the Amazon Resource Name \(ARN\) of your IAM role\.
     + Use existing role – Choose one of your existing roles from the list\.
   + \(Optional\) Image tags – Choose **Add new tag**\. You can add up to 50 tags\. Tags are searchable using the Studio user interface, the SageMaker console, or the SageMaker `Search` API\.

1. Choose **Submit**\.

The new image is displayed in the **Custom images** list and briefly highlighted\. After the image has been successfully created, you can choose the image name to view its properties or choose **Create version** to create another version\.

**To create another image version**

1. Choose **Create version** on the same row as the image\.

1. For **Image source**, enter the registry path to the Amazon ECR container image\. The container image shouldn't be the same image as used in a previous version of the SageMaker image\.

## Create a SageMaker image from the AWS CLI<a name="studio-byoi-sdk-create-image"></a>

You perform the following steps to create a SageMaker image from the container image using the AWS CLI\.
+ Create an `Image`\.
+ Create an `ImageVersion`\.
+ Create a configuration file\.
+ Create an `AppImageConfig`\.

**To create the SageMaker image entities**

1. Create a SageMaker image\.

   ```
   aws sagemaker create-image \
       --image-name custom-image \
       --role-arn arn:aws:iam::<acct-id>:role/service-role/<execution-role>
   ```

   The response should look similar to the following\.

   ```
   {
       "ImageArn": "arn:aws:sagemaker:us-east-2:acct-id:image/custom-image"
   }
   ```

1. Create a SageMaker image version from the container image\.

   ```
   aws sagemaker create-image-version \
       --image-name custom-image \
       --base-image <acct-id>.dkr.ecr.<region>.amazonaws.com/smstudio-custom:custom-image
   ```

   The response should look similar to the following\.

   ```
   {
       "ImageVersionArn": "arn:aws:sagemaker:us-east-2:acct-id:image-version/custom-image/1"
   }
   ```

1. Check that the image version was successfully created\.

   ```
   aws sagemaker describe-image-version \
       --image-name custom-image \
       --version-number 1
   ```

   The response should look similar to the following\.

   ```
   {
       "ImageVersionArn": "arn:aws:sagemaker:us-east-2:acct-id:image-version/custom-image/1",
       "ImageVersionStatus": "CREATED"
   }
   ```
**Note**  
If the response is `"ImageVersionStatus": "CREATED_FAILED"`, the response also includes the failure reason\. A permissions issue is a common cause of failure\. You also can check your Amazon CloudWatch logs if you experience a failure when starting or running the KernelGateway app for a custom image\. The name of the log group is `/aws/sagemaker/studio`\. The name of the log stream is `$domainID/$userProfileName/KernelGateway/$appName`\.

1. Create a configuration file, named `app-image-config-input.json`\. The `Name` value of `KernelSpecs` must match the name of the kernelSpec available in the Image associated with this `AppImageConfig`\. This value is case sensitive\. You can find the available kernelSpecs in an image by running `jupyter-kernelspec list` from a shell inside the container\. `MountPath` is the path within the image to mount your Amazon Elastic File System \(Amazon EFS\) home directory\. It needs to be different from the path you use inside the container because that path will be overridden when your Amazon EFS home directory is mounted\.
**Note**  
The following `DefaultUID` and `DefaultGID` combinations are the only accepted values:   
 DefaultUID: 1000 and DefaultGID: 100 
 DefaultUID: 0 and DefaultGID: 0 

   ```
   {
       "AppImageConfigName": "custom-image-config",
       "KernelGatewayImageConfig": {
           "KernelSpecs": [
               {
                   "Name": "python3",
                   "DisplayName": "Python 3 (ipykernel)"
               }
           ],
           "FileSystemConfig": {
               "MountPath": "/home/sagemaker-user",
               "DefaultUid": 1000,
               "DefaultGid": 100
           }
       }
   }
   ```

1. Create the AppImageConfig using the file created in the previous step\.

   ```
   aws sagemaker create-app-image-config \
       --cli-input-json file://app-image-config-input.json
   ```

   The response should look similar to the following\.

   ```
   {
       "AppImageConfigArn": "arn:aws:sagemaker:us-east-2:acct-id:app-image-config/custom-image-config"
   }
   ```