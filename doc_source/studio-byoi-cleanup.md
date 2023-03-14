# Clean up resources<a name="studio-byoi-cleanup"></a>

The following sections show how to clean up the resources you created in the previous sections from the SageMaker console or AWS CLI\. You perform the following steps to clean up the resources:
+ Detach the image and image versions from your domain\.
+ Delete the image, image version, and app image config\.
+ Delete the container image and repository from Amazon ECR\. For more information, see [Deleting a repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-delete.html)\.

## Clean up resources from the SageMaker console<a name="studio-byoi-detach"></a>

The following section shows how to clean up resources from the SageMaker console\.

When you detach an image from a domain, all versions of the image are detached\. When an image is detached, all users of the domain lose access to the image versions\. A running notebook that has a kernel session on an image version when the version is detached, continues to run\. When the notebook is stopped or the kernel is shut down, the image version becomes unavailable\.

**To detach an image**

1. In the **Control Panel**, under **Custom SageMaker Studio images attached to domain**, choose the image and then choose **Detach**\.

1. \(Optional\) To delete the image and all versions from SageMaker, select **Also delete the selected images \.\.\.**\. This does not delete the associated container images from Amazon ECR\.

1. Choose **Detach**\.

## Clean up resources from the AWS CLI<a name="studio-byoi-sdk-cleanup"></a>

The following section shows how to clean up resources from the AWS CLI\.

**To clean up resources**

1. Detach the image and image versions from your domain by passing an empty custom image list to the domain\. Open the `default-user-settings.json` file you created in [Attach the SageMaker image to your current domain](studio-byoi-attach.md#studio-byoi-sdk-attach-current-domain)\. To detach the image and image version from a shared space, open the `default-space-settings.json` file\.

1. Delete the custom images and then save the file\.

   ```
   "DefaultUserSettings": {
     "KernelGatewayAppSettings": {
        "CustomImages": [
        ],
        ...
     },
     ...
   }
   ```

1. Use the domain ID and default user settings file to update your domain\. To update your shared space, use the default space settings file\.

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

1. Delete the app image config\.

   ```
   aws sagemaker delete-app-image-config \
       --app-image-config-name custom-image-config
   ```

1. Delete the SageMaker image, which also deletes all image versions\. The container images in ECR that are represented by the image versions are not deleted\.

   ```
   aws sagemaker delete-image \
       --image-name custom-image
   ```