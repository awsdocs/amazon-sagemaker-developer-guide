# Clean up resources<a name="studio-byoi-sdk-cleanup"></a>

You perform the following steps to clean up the resources you created in the previous sections:
+ Detach the image and image versions from your domain\.
+ Delete the image, image version, and app image config\.
+ Delete the container image and repository from Amazon ECR\. For more information, see [Deleting a repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-delete.html)\.

**To clean up resources**

1. Detach the image and image versions from your domain by passing an empty custom image list to the domain\. Open the `default-user-settings.json` file you created in [Attach the image to a current domain](studio-byoi-sdk-attach-current-domain.md)\.

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

1. Delete the app image config\.

   ```
   aws sagemaker delete-app-image-config \
       --app-image-config-name r-image-config
   ```

1. Delete the SageMaker image, which also deletes all image versions\. The container images in ECR that are represented by the image versions are not deleted\.

   ```
   aws sagemaker delete-image \
       --image-name r-image
   ```