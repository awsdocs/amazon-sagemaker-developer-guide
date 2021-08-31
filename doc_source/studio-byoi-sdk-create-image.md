# Create a SageMaker image from the ECR container image<a name="studio-byoi-sdk-create-image"></a>

You perform the following steps to create a SageMaker image from the container image:
+ Create an `Image`\.
+ Create an `ImageVersion`\.
+ Create a configuration file\.
+ Create an `AppImageConfig`\.

**To create the SageMaker image entities**

1. Create a SageMaker image\.

   ```
   aws sagemaker create-image \
       --image-name r-image \
       --role-arn arn:aws:iam::<acct-id>:role/service-role/<execution-role>
   ```

   Response:

   ```
   {
       "ImageArn": "arn:aws:sagemaker:us-east-2:acct-id:image/r-image"
   }
   ```

1. Create a SageMaker image version from the container image\.

   ```
   aws sagemaker create-image-version \
       --image-name r-image \
       --base-image <acct-id>.dkr.ecr.<region>.amazonaws.com/smstudio-custom:r
   ```

   Response:

   ```
   {
       "ImageVersionArn": "arn:aws:sagemaker:us-east-2:acct-id:image-version/r-image/1"
   }
   ```

1. Check that the image version was successfully created\.

   ```
   aws sagemaker describe-image-version \
       --image-name r-image \
       --version 1
   ```

   Response:

   ```
   {
       "ImageVersionArn": "arn:aws:sagemaker:us-east-2:acct-id:image-version/r-image/1",
       "ImageVersionStatus": "CREATED"
   }
   ```
**Note**  
If the response is `"ImageVersionStatus": "CREATED_FAILED"`, the response also includes the failure reason\. A permissions issue is a common cause of failure\. You also can check your Amazon CloudWatch logs\. The name of the log group is `/aws/sagemaker/studio`\. The name of the log stream is `$domainID/$userProfileName/KernelGateway/$appName`\.

1. Create a configuration file, named `app-image-config-input.json`\. The `Name` value of `KernelSpecs` must match the name of the kernelSpec available in the Image associated with this `AppImageConfig`\. This value is case sensitive\. You can find the available kernelSpecs in an image by running `jupyter-kernelspec list` from a shell inside the container\. `MountPath` is the path within the image to mount your Amazon Elastic File System \(Amazon EFS\) home directory\. It needs to be different from the path you use inside the container because that path will be overridden when your Amazon EFS home directory is mounted\. For information on testing your image locally before using it in Studio, see [DEVELOPMENT](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/blob/main/DEVELOPMENT.md) in the SageMaker Studio Custom Image Samples repository\. 
**Note**  
The following `DefaultUID` and `DefaultGID` combinations are the only accepted values:   
 DefaultUID: 1000 and DefaultGID: 100 
 DefaultUID: 0 and DefaultGID: 0 

   ```
   {
       "AppImageConfigName": "r-image-config",
       "KernelGatewayImageConfig": {
           "KernelSpecs": [
               {
                   "Name": "ir",
                   "DisplayName": "R (Custom R Image)"
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

   Response:

   ```
   {
       "AppImageConfigArn": "arn:aws:sagemaker:us-east-2:acct-id:app-image-config/r-image-config"
   }
   ```