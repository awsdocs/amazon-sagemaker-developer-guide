# Attach a custom SageMaker image<a name="studio-byoi-attach"></a>

To use a custom SageMaker image, you must attach a version of the image to your domain\. When you attach an image version, it appears in the SageMaker Studio Launcher and is available in the **Select image** dropdown list, which users use to launch an activity or change the image used by a notebook\.

To make a custom SageMaker image available to all users within a domain, you attach the image to the domain\. To make an image available to a single user, you attach the image to the user's profile\. When you attach an image, SageMaker uses the latest image version by default\. You can also attach a specific image version\. After you attach the version, you can choose the version from the SageMaker Launcher or the image selector when you launch a notebook\.

There is a limit to the number of image versions that can be attached at any given time\. After you reach the limit, you must detach a version in order to attach another version of the image\.

The following sections demonstrate how to attach a custom SageMaker image to your domain using either the SageMaker console or the AWS CLI\.

## Attach the SageMaker image using the Console<a name="studio-byoi-attach-existing"></a>

This topic describes how you can attach an existing custom SageMaker image version to your domain using the SageMaker control panel\. You can also create a custom SageMaker image and image version, and then attach that version to your domain\. For the procedure to create an image and image version, see [Create a custom SageMaker image](studio-byoi-create.md)\.

**To attach an existing image**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Domains**\.

1. From the **Domains** page, select the Domain to attach the image to\.

1. From the **Domain details** page, select the **Environment** tab\.

1. On the **Environment** tab, under **Custom SageMaker Studio images attached to domain**, choose **Attach image**\.

1. For **Image source**, choose **Existing image**\.

1. Choose an existing image from the list\.

1. Choose a version of the image from the list\.

1. Choose **Next**\.

1. Verify the values for **Image name**, **Image display name**, and **Description**\.

1. Choose the IAM role\. For more information, see [Create a custom SageMaker image](studio-byoi-create.md)\.

1. \(Optional\) Add tags for the image\.

1. Specify the EFS mount path\. This is the path within the image to mount the user's Amazon Elastic File System \(EFS\) home directory\.

1. For **Image type**, select **SageMaker Studio image**

1. For **Kernel name**, enter the name of an existing kernel in the image\. For information on how to get the kernel information from the image, see [DEVELOPMENT](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/blob/main/DEVELOPMENT.md) in the SageMaker Studio Custom Image Samples repository\. For more information, see the **Kernel discovery** and **User data** sections of [Custom SageMaker image specifications](studio-byoi-specs.md)\.

1. \(Optional\) For **Kernel display name**, enter the display name for the kernel\.

1. Choose **Add kernel**\.

1. Choose **Submit**\. 

   1. Wait for the image version to be attached to the domain\. When attached, the version is displayed in the **Custom images** list and briefly highlighted\.

## Attach the SageMaker image using the AWS CLI<a name="studio-byoi-sdk-attach"></a>

The following sections demonstrate how to attach a custom SageMaker image when creating a new domain or updating your existing domain using the AWS CLI\.

### Attach the SageMaker image to a new domain<a name="studio-byoi-sdk-attach-new-domain"></a>

The following section demonstrates how to create a new domain with the version attached\. These steps require that you specify the Amazon Virtual Private Cloud \(VPC\) information and execution role required to create the domain\. You perform the following steps to create the domain and attach the custom SageMaker image:
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

   The response should look similar to the following\.

   ```
   vpc-xxxxxxxx
   ```

1. Get your default subnet IDs using the VPC ID from the previous step\.

   ```
   aws ec2 describe-subnets \
       --filters Name=vpc-id,Values=<vpc-id> \
       --query "Subnets[*].SubnetId" --output json
   ```

   The response should look similar to the following\.

   ```
   [
       "subnet-b55171dd",
       "subnet-8a5f99c6",
       "subnet-e88d1392"
   ]
   ```

1. Create a configuration file named `create-domain-input.json`\. Insert the VPC ID, subnet IDs, `ImageName`, and `AppImageConfigName` from the previous steps\. Because `ImageVersionNumber` isn't specified, the latest version of the image is used, which is the only version in this case\.

   ```
   {
       "DomainName": "domain-with-custom-image",
       "VpcId": "<vpc-id>",
       "SubnetIds": [
           "<subnet-ids>"
       ],
       "DefaultUserSettings": {
           "ExecutionRole": "<execution-role>",
           "KernelGatewayAppSettings": {
               "CustomImages": [
                   {
                       "ImageName": "custom-image",
                       "AppImageConfigName": "custom-image-config"
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

   The response should look similar to the following\.

   ```
   {
       "DomainArn": "arn:aws:sagemaker:us-east-2:acct-id:domain/d-xxxxxxxxxxxx",
       "Url": "https://d-xxxxxxxxxxxx.studio.us-east-2.sagemaker.aws/..."
   }
   ```

### Attach the SageMaker image to your current domain<a name="studio-byoi-sdk-attach-current-domain"></a>

If you have onboarded to a SageMaker domain, you can attach the custom image to your current domain\. For more information about onboarding to a SageMaker domain, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\. You don't need to specify the VPC information and execution role when attaching a custom image to your current domain\. After you attach the version, you must delete all the apps in your domain and reopen Studio\. For information about deleting the apps, see [Delete an Amazon SageMaker Domain](gs-studio-delete-domain.md)\.

You perform the following steps to add the SageMaker image to your current domain\.
+ Get your `DomainID` from SageMaker control panel\.
+ Use the `DomainID` to get the `DefaultUserSettings` for the domain\.
+ Add the `ImageName` and `AppImageConfig` as a `CustomImage` to the `DefaultUserSettings`\.
+ Update your domain to include the custom image\.

**To add the custom SageMaker image to your domain**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Domains**\.

1. From the **Domains** page, select the Domain to attach the image to\.

1. From the **Domain details** page, select the **Domain settings** tab\.

1. From the **Domain settings** tab, under **General settings**, find the `DomainId`\. The ID is in the following format: `d-xxxxxxxxxxxx`\.

1. Use the domain ID to get the description of the domain\.

   ```
   aws sagemaker describe-domain \
       --domain-id <d-xxxxxxxxxxxx>
   ```

   The response should look similar to the following\.

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

1. Save the default user settings section of the response to a file named `default-user-settings.json`\.

1. Insert the `ImageName` and `AppImageConfigName` from the previous steps as a custom image\. Because `ImageVersionNumber` isn't specified, the latest version of the image is used, which is the only version in this case\.

   ```
   {
       "DefaultUserSettings": {
           "KernelGatewayAppSettings": { 
              "CustomImages": [ 
                 { 
                    "ImageName": "string",
                    "AppImageConfigName": "string"
                 }
              ],
              ...
           }
       }
   }
   ```

1. Use the domain ID and default user settings file to update your domain\.

   ```
   aws sagemaker update-domain \
       --domain-id <d-xxxxxxxxxxxx> \
       --cli-input-json file://default-user-settings.json
   ```

   The response should look similar to the following\.

   ```
   {
       "DomainArn": "arn:aws:sagemaker:us-east-2:acct-id:domain/d-xxxxxxxxxxxx"
   }
   ```

## View the attached image in the SageMaker control panel<a name="studio-byoi-sdk-view"></a>

After you create the custom SageMaker image and attach it to your domain, the image appears in the custom images list in the control panel\.