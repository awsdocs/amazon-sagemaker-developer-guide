# Attach a custom SageMaker image<a name="rstudio-byoi-attach"></a>

This guide shows how to attach a custom RStudio image to your Amazon SageMaker Domain using the SageMaker console or the AWS Command Line Interface \(AWS CLI\)\. 

To use a custom SageMaker image, you must attach a custom RStudio image to your Domain\. When you attach an image version, it appears in the RStudio Launcher and is available in the **Select image** dropdown list\. You use the dropdown to change the image used by RStudio\.

There is a limit to the number of image versions that you can attach\. After you reach the limit, you must first detach a version so that you can attach a different version of the image\.

**Topics**
+ [Attach an image version to your Domain using the console](#rstudio-byoi-attach-console)
+ [Attach an existing image version to your Domain using the AWS CLI](#rstudio-byoi-attach-cli)

## Attach an image version to your Domain using the console<a name="rstudio-byoi-attach-console"></a>

You can attach a custom SageMaker image version to your Domain using the SageMaker console's control panel\. You can also create a custom SageMaker image, and an image version, and then attach that version to your Domain\.

**To attach an existing image**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Domains**\.

1. Select the desired Domain\.

1. Choose **Environment**\.

1. Under **Custom SageMaker Studio images attached to domain**, choose **Attach image**\.

1. For **Image source**, choose **Existing image** or **New image**\.

   If you select **Existing image**, choose an image from the Amazon SageMaker image store\.

   If you select **New image**, provide the Amazon ECR registry path for your Docker image\. The path must be in the same AWS Region as the Domain\. The Amazon ECR repo must be in the same account as your Domain, or cross\-account permissions for SageMaker must be enabled\.

1. Choose an existing image from the list\.

1. Choose a version of the image from the list\.

1. Choose **Next**\.

1. Enter values for **Image name**, **Image display name**, and **Description**\.

1. Choose the IAM role\. For more information, see [Create a custom RStudio image](rstudio-byoi-create.md)\.

1. \(Optional\) Add tags for the image\.

1. \(Optional\) Choose **Add new tag**, then add a configuration tag\.

1. For **Image type**, select **RStudio Image**\.

1. Choose **Submit**\.

Wait for the image version to be attached to the Domain\. After the version is attached, it appears in the **Custom images** list and is briefly highlighted\.

## Attach an existing image version to your Domain using the AWS CLI<a name="rstudio-byoi-attach-cli"></a>

Two methods are presented to attach the image version to your Domain using the AWS CLI\. In the first method, you create a new Domain with the version attached\. This method is simpler but you must specify the Amazon Virtual Private Cloud \(Amazon VPC\) information and execution role that's required to create the Domain\.

If you have already onboarded to the Domain, you can use the second method to attach the image version to your current Domain\. In this case, you don't need to specify the Amazon VPC information and execution role\. After you attach the version, delete all of the applications in your Domain and relaunch RStudio\.

### Attach the SageMaker image to a new Domain<a name="rstudio-byoi-cli-attach-new-domain"></a>

To use this method, you must specify an execution role that has the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

Use the following steps to create the Domain and attach the custom SageMaker image:
+ Get your default VPC ID and subnet IDs\.
+ Create the configuration file for the Domain, which specifies the image\.
+ Create the Domain with the configuration file\.

**To add the custom SageMaker image to your Domain**

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

1. Create the Domain with the attached custom SageMaker image\.

   ```
   aws sagemaker create-domain \
       --cli-input-json file://create-domain-input.json
   ```

   Response:

   ```
   {
       "DomainArn": "arn:aws:sagemaker:region:acct-id:domain/domain-id",
       "Url": "https://domain-id.studio.region.sagemaker.aws/..."
   }
   ```

### Attach the SageMaker image to an existing Domain<a name="rstudio-byoi-cli-attach-current-domain"></a>

This method assumes that you've already onboarded to Domain\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

**Note**  
You must delete all of the applications in your Domain to update the Domain with the new image version\. For information about deleting these applications, see [Delete an Amazon SageMaker Domain](gs-studio-delete-domain.md)\.

Use the following steps to add the SageMaker image to your current Domain\.
+ Get your `DomainID` from the SageMaker console\.
+ Use the `DomainID` to get the `DefaultUserSettings` for the Domain\.
+ Add the `ImageName` and `AppImageConfig` as a `CustomImage` to the `DefaultUserSettings`\.
+ Update your Domain to include the custom image\.

**To add the custom SageMaker image to your Domain**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. From the left navigation pane, choose **Domains**\.

1. Select the desired Domain\.

1. Choose **Domain settings**\.

1. Under **General Settings**, find the **Domain ID**\. The ID is in the following format: `d-xxxxxxxxxxxx`\.

1. Use the Domain ID to get the description of the Domain\.

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

1. Use the Domain ID and default user settings file to update your Domain\.

   ```
   aws sagemaker update-domain \
       --domain-id <d-xxxxxxxxxxxx> \
       --cli-input-json file://update-domain-input.json
   ```

   Response:

   ```
   {
       "DomainArn": "arn:aws:sagemaker:region:acct-id:domain/domain-id"
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