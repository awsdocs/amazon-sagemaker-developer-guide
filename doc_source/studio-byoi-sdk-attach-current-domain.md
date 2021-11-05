# Attach the SageMaker image to your current domain<a name="studio-byoi-sdk-attach-current-domain"></a>

This method presumes you've already onboarded to Amazon SageMaker Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

**Note**  
You must delete all the apps in your domain before you update the domain with the new image version\. For information about deleting the apps, see [Delete an Amazon SageMaker Domain](gs-studio-delete-domain.md)\.

You perform the following steps to add the SageMaker image to your current domain\.
+ Get your `DomainID` from SageMaker Studio\.
+ Use the `DomainID` to get the `DefaultUserSettings` for the domain\.
+ Add the `ImageName` and `AppImageConfig` as a `CustomImage` to the `DefaultUserSettings`\.
+ Update your domain to include the custom image\.

**To add the custom SageMaker image to your domain**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. From the top left of the navigation pane, choose **Amazon SageMaker Studio**\.

1. From the Studio Control Panel, under **Studio Summary**, find the **Studio ID**, which is also your `DomainId`\. The ID is in the following format: `d-xxxxxxxxxxxx`\.  
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

   Response:

   ```
   {
       "DomainArn": "arn:aws:sagemaker:us-east-2:acct-id:domain/d-xxxxxxxxxxxx"
   }
   ```