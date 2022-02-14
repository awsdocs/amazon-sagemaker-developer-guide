# Delete an Amazon SageMaker Domain<a name="gs-studio-delete-domain"></a>

When you onboard to Amazon SageMaker Domain using IAM authentication, Amazon SageMaker creates a domain for your account\. A domain consists of a list of authorized users, configuration settings, and an Amazon Elastic File System \(Amazon EFS\) volume, which contains data for the users, including notebooks, resources, and artifacts\. A user can have multiple applications \(apps\) which support the reading and execution experience of the user’s notebooks, terminals, and consoles\. 

Your files are kept in the Amazon EFS volume as a backup\. This backup includes the files in the mounted directory, which is `/home/sagemaker-user` for Jupyter and `/root` for your kernel\. When you delete files from these mounted directories, the kernel or app may move the deleted files into a hidden trash folder\. If the trash folder is inside the mounted directory, those files are copied into the Amazon EFS volume and will incur charges\. To avoid these Amazon EFS charges, you must identify and clean the trash folder location\. The trash folder location for default apps and kernels is `~/.local/`\. This may vary depending on the Linux distribution used for custom apps or kernels\. For more information about the Amazon EFS volume, see [Manage Your EFS Storage Volume in SageMaker Studio](studio-tasks-manage-storage.md)\.

To return Amazon SageMaker to the state it was in before you onboarded, you must delete this domain\. You can delete the domain by using the Amazon SageMaker Domain Control Panel, the AWS Command Line Interface \(AWS CLI\), or the SageMaker SDK\. When you use the Amazon SageMaker Domain Control Panel to delete the domain, the Amazon EFS volume is detached but not deleted\. The same behavior occurs by default when you use the AWS CLI or the SDK to delete the domain\. However, when you use the AWS CLI or the SDK, you can set the `RetentionPolicy` to `HomeEfsFileSystem=Delete` to delete the EFS volume along with the domain\.

You must delete the domain if you want to switch authentication modes from IAM to AWS SSO\.

To delete a domain, the domain cannot contain any user profiles\. To delete a user profile, the profile cannot contain any non\-failed apps\.

When you delete these resources, the following occurs:
+ App – The data \(files and notebooks\) in a user's home directory is saved\. Unsaved notebook data is lost\.
+ User profile – The user is no longer able to sign in to the Domain and loses access to their home directory, but the data is not deleted\. An admin can retrieve the data from the Amazon EFS volume where it is stored under the user's AWS account\.

**Note**  
You must have admin permission to delete a Domain\.

You can only delete an app whose status is `InService`, which is displayed as **Ready** in the Domain\. An app whose status is `Failed` doesn't need to be deleted to delete the containing domain\. In the Domain, an attempt to delete an app in the failed state results in an error\.

**Topics**
+ [Delete a Amazon SageMaker Domain \(Console\)](#gs-studio-delete-domain-studio)
+ [Delete a Amazon SageMaker Domain \(CLI\)](#gs-studio-delete-domain-cli)

## Delete a Amazon SageMaker Domain \(Console\)<a name="gs-studio-delete-domain-studio"></a>

**To delete a domain**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **SageMaker Domain** on the left side of the page to open the **SageMaker Domain Control Panel**\.

1. Repeat the following steps for each user in the **User name** list\.

   1. Choose the user\.

   1. On the **User Details** page, for each non\-failed app in the **Apps** list, choose **Delete app**\.

   1. On the **Delete app** dialog, choose **Yes, delete app**, type *delete* in the confirmation field, and then choose **Delete**\.

   1. When the **Status** for all apps show as **Deleted**, choose **Edit**\.

   1. From the **Edit User** page, choose **Delete user**\.

   1. On the **Delete user** dialog, choose **Yes, delete user**, type *delete* in the confirmation field, and then choose **Delete**\.
**Important**  
When a user is deleted, they lose access to the Amazon EFS volume that contains their data, including notebooks and other artifacts\. The data is not deleted and can be accessed by an administrator\.

1. When all users are deleted, choose **Delete Domain**\.

1. On the **Delete Domain** dialog, choose **Yes, delete Domain**, type *delete* in the confirmation field, and then choose **Delete**\.

## Delete a Amazon SageMaker Domain \(CLI\)<a name="gs-studio-delete-domain-cli"></a>

**To delete a domain**

1. Retrieve the list of domains in your account\.

   ```
   aws --region Region sagemaker list-domains
   ```

1. Retrieve the list of applications for the domain to be deleted\.

   ```
   aws --region Region sagemaker list-apps \
       --domain-id-equals DomainId
   ```

1. Delete each application in the list\.

   ```
   aws --region Region sagemaker delete-app \
       --domain-id DomainId \
       --app-name AppName \
       --app-type AppType \
       --user-profile-name UserProfileName
   ```

1. Retrieve the list of user profiles in the domain\.

   ```
   aws --region Region sagemaker list-user-profiles \
       --domain-id-equals DomainId
   ```

1. Delete each user profile in the list\.

   ```
   aws --region Region sagemaker delete-user-profile \
       --domain-id DomainId \
       --user-profile-name UserProfileName
   ```

1. Delete the domain\. To also delete the Amazon EFS volume, specify `HomeEfsFileSystem=Delete`\.

   ```
   aws --region Region sagemaker delete-domain \
       --domain-id DomainId \
       --retention-policy HomeEfsFileSystem=Retain
   ```