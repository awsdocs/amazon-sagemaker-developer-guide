# Attach a custom SageMaker image<a name="rstudio-byoi-attach"></a>

This guide shows how to attach a custom RStudio image to your domain using the SageMaker console or the AWS Command Line Interface \(AWS CLI\)\. 

To use a custom SageMaker image, you must attach a custom RStudio image to your domain\. When you attach an image version, it appears in the RStudio Launcher and is available in the **Select image** dropdown list\. You use the dropdown to change the image used by RStudio\.

There is a limit to the number of image versions that you can attach\. After you reach the limit, you must first detach a version so that you can attach a different version of the image\.

**Topics**
+ [Attach an image version to your domain using the console](#rstudio-byoi-attach-console)
+ [Attach an existing image version to your domain using the AWS CLI](#rstudio-byoi-attach-cli)

## Attach an image version to your domain using the console<a name="rstudio-byoi-attach-console"></a>

You can attach a custom SageMaker image version to your domain using the SageMaker console's control panel\. You can also create a custom SageMaker image, and an image version, and then attach that version to your domain\.

**To attach an existing image**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Control panel**\.

1. On the **SageMaker Control Panel**, under **Custom SageMaker Studio images attached to domain**, choose **Attach image**\.

1. For **Image source**, choose **Existing image** or **New image**\.

   If you select **Existing image**, choose an image from the Amazon SageMaker image store\.

   If you select **New image**, provide the Amazon ECR registry path for your Docker image\. The path must be in the same AWS Region as the domain\. The Amazon ECR repo must be in the same account as your domain, or cross\-account permissions for SageMaker must be enabled\.

1. Choose an existing image from the list\.

1. Choose a version of the image from the list\.

1. Choose **Next**\.

1. Enter values for **Image name**, **Image display name**, and **Description**\.

1. Choose the IAM role\. For more information, see [Create a custom RStudio image](rstudio-byoi-create.md)\.

1. \(Optional\) Add tags for the image\.

1. \(Optional\) Choose **Add new tag**, then add a configuration tag\.

1. For **Image type**, select **RStudio Image**\.

1. Choose **Submit**\.

Wait for the image version to be attached to the domain\. After the version is attached, it appears in the **Custom images** list and is briefly highlighted\.

## Attach an existing image version to your domain using the AWS CLI<a name="rstudio-byoi-attach-cli"></a>

Two methods are presented to attach the image version to your domain using the AWS CLI\. In the first method, you create a new domain with the version attached\. This method is simpler but you must specify the Amazon Virtual Private Cloud \(Amazon VPC\) information and execution role that's required to create the domain\.

If you have already onboarded to the SageMaker domain, you can use the second method to attach the image version to your current domain\. In this case, you don't need to specify the Amazon VPC information and execution role\. After you attach the version, delete all of the applications in your domain and relaunch RStudio\.

### Attach the SageMaker image to a new domain<a name="rstudio-byoi-cli-attach-new-domain"></a>

To use this method, you must specify an execution role that has the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

**Note**  
You can have only one domain\. If you have onboarded to a SageMaker domain, you must delete your current domain before you can use this method\. For more information, see [Delete an Amazon SageMaker Domain](gs-studio-delete-domain.md)\.

Use the following steps to create the domain and attach the custom SageMaker image:
+ Get your default VPC ID and subnet IDs\.
+ Create the configuration file for the domain, which specifies the image\.
+ Create the domain with the configuration file\.

**To add the custom SageMaker image to your domain**

1. Get your default VPC ID\.

   ```
   aws ec2 describe-vpcs \
       --filters Name=isDefault,Values=true \
       --query "Vpcs[0].VpcId" --output text
   ```

   Response:

   ```
   vpc-xxxxxxxx
   ```

1. Get your default subnet IDs using the VPC ID from the previous step\.

   ```
   aws ec2 describe-subnets \
       --filters Name=vpc-id,Values=<vpc-id> \
       --query "Subnets[*].SubnetId" --output json
   ```

   Response:

   ```
   [
       "subnet-b55171dd",
       "subnet-8a5f99c6",
       "subnet-e88d1392"
   ]
   ```

1. Create a configuration file named `create-domain-input.json`\. Insert the VPC ID, subnet IDs, `ImageName`, and `AppImageConfigName` from the previous steps\. Because `ImageVersionNumber` isn't specified, the latest version of the image is used, which is the only version in this case\. Your execution role must satisfy the requirements in [Prerequisites](rstudio-byoi-prerequisites.md)\.

   ```
   {
     "DomainName": "domain-with-custom-r-image",
     "VpcId": "<vpc-id>",
     "SubnetIds": [
       "<subnet-ids>"
     ],
     "DomainSettings": {
       "RStudioServerProDomainSettings": {
         "DomainExecutionRoleArn": "<execution-role>"
       }
     },
     "DefaultUserSettings": {
       "ExecutionRole": "<execution-role>",
       "RSessionAppSettings": {
         "CustomImages": [
           {
            "AppImageConfigName": "rstudio-custom-config",
            "ImageName": "rstudio-custom-image"
           }
         ]
        }
     },
     "AuthMode": "IAM"
   }
   ```

1. Create the domain with the attached custom SageMaker image\.

   ```
   aws sagemaker create-domain \
       --cli-input-json file://create-domain-input.json
   ```

   Response:

   ```
   {
       "DomainArn": "arn:aws:sagemaker:us-east-2:acct-id:domain/d-xxxxxxxxxxxx",
       "Url": "https://d-xxxxxxxxxxxx.studio.us-east-2.sagemaker.aws/..."
   }
   ```

### Attach the SageMaker image to an existing domain<a name="rstudio-byoi-cli-attach-current-domain"></a>

This method assumes that you've already onboarded to Amazon SageMaker domain\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

**Note**  
You must delete all of the applications in your domain before you update the domain with the new image version\. For information about deleting these applications, see [Delete an Amazon SageMaker Domain](gs-studio-delete-domain.md)\.

Use the following steps to add the SageMaker image to your current domain\.
+ Get your `DomainID` from the SageMaker console\.
+ Use the `DomainID` to get the `DefaultUserSettings` for the domain\.
+ Add the `ImageName` and `AppImageConfig` as a `CustomImage` to the `DefaultUserSettings`\.
+ Update your domain to include the custom image\.

**To add the custom SageMaker image to your domain**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. From the left navigation pane, choose **Control panel**\.

1. From the **Control panel**, under **Domain**, find the **Domain ID**\. The ID is in the following format: `d-xxxxxxxxxxxx`\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-byoi-id.png)

1. Use the domain ID to get the description of the domain\.

   ```
   aws sagemaker describe-domain \
       --domain-id <d-xxxxxxxxxxxx>
   ```

   Response:

   ```
   {
       "DomainId": "d-xxxxxxxxxxxx",
       "DefaultUserSettings": {
         "KernelGatewayAppSettings": {
           "CustomImages": [
           ],
           ...
         }
       }
   }
   ```

1. Save the `DefaultUserSettings` section of the response to a file named `update-domain-input.json`\.

1. Insert the `ImageName` and `AppImageConfigName` from the previous steps as a custom image\. Because `ImageVersionNumber` isn't specified, the latest version of the image is used, which is the only version in this case\.

   ```
   {
       "DefaultUserSettings": {
           "RSessionAppSettings": { 
              "CustomImages": [ 
                 { 
                    "ImageName": "rstudio-custom-image",
                    "AppImageConfigName": "rstudio-custom-config"
                 }
              ]
           }
       }
   }
   ```

1. Use the domain ID and default user settings file to update your domain\.

   ```
   aws sagemaker update-domain \
       --domain-id <d-xxxxxxxxxxxx> \
       --cli-input-json file://update-domain-input.json
   ```

   Response:

   ```
   {
       "DomainArn": "arn:aws:sagemaker:us-east-2:acct-id:domain/d-xxxxxxxxxxxx"
   }
   ```

1. Delete the `RStudioServerPro` application\. You must restart the `RStudioServerPro` domain\-shared application for the RStudio Launcher UI to pick up the latest changes\.

   ```
   aws sagemaker delete-app \
       --domain-id <d-xxxxxxxxxxxx> --user-profile-name domain-shared \
       --app-type RStudioServerPro --app-name default
   ```

1. Create a new `RStudioServerPro` application\. You must create this application using the AWS CLI\.

   ```
   aws sagemaker create-app \
       --domain-id <d-xxxxxxxxxxxx> --user-profile-name domain-shared \
       --app-type RStudioServerPro --app-name default
   ```