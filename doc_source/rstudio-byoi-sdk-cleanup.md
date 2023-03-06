# Clean up image resources<a name="rstudio-byoi-sdk-cleanup"></a>

This guide shows how to clean up RStudio image resources that you created in the previous sections\. To delete an image, complete the following steps using either the SageMaker console or the AWS CLI, as shown in this guide\.
+ Detach the image and image versions from your Amazon SageMaker Domain\.
+ Delete the image, image version, and app image config\.

After you've completed these steps, you can delete the container image and repository from Amazon ECR\. For more information about how to delete the container image and repository, see [Deleting a repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-delete.html)\.

## Clean up resources from the SageMaker console<a name="rstudio-byoi-sdk-cleanup-console"></a>

When you detach an image from a Domain, all versions of the image are detached\. When an image is detached, all users of the Domain lose access to the image versions\.

**To detach an image**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. From the left navigation pane, choose **Domains**\.

1. Select the desired Domain\.

1. Choose **Environment**\.

1. Under **Custom images attached to domain**, choose the image and then choose **Detach**\.

1. \(Optional\) To delete the image and all versions from SageMaker, select **Also delete the selected images \.\.\.**\. This does not delete the associated images from Amazon ECR\.

1. Choose **Detach**\.

## Clean up resources from the AWS CLI<a name="rstudio-byoi-sdk-cleanup-cli"></a>

**To clean up resources**

1. Detach the image and image versions from your Domain by passing an empty custom image list to the Domain\. Open the `update-domain-input.json` file that you created in [Attach the SageMaker image to your current domain](studio-byoi-attach.md#studio-byoi-sdk-attach-current-domain)\.

1. Delete the `RSessionAppSettings` custom images and then save the file\. Do not modify the `KernelGatewayAppSettings` custom images\.

   ```
   {
       "DomainId": "d-xxxxxxxxxxxx",
       "DefaultUserSettings": {
         "KernelGatewayAppSettings": {
            "CustomImages": [
            ],
            ...
         },
         "RSessionAppSettings": { 
           "CustomImages": [ 
           ],
           "DefaultResourceSpec": { 
           }
           ...
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
       "DomainArn": "arn:aws:sagemaker:us-east-2:acct-id:domain/d-xxxxxxxxxxxx"
   }
   ```

1. Delete the app image config\.

   ```
   aws sagemaker delete-app-image-config \
       --app-image-config-name rstudio-image-config
   ```

1. Delete the SageMaker image, which also deletes all image versions\. The container images in Amazon ECR that are represented by the image versions are not deleted\.

   ```
   aws sagemaker delete-image \
       --image-name rstudio-image
   ```