# Delete an Amazon SageMaker Studio Domain<a name="gs-studio-delete-domain"></a>

When you onboard to Amazon SageMaker Studio using IAM authentication, Studio creates a domain for your account\. A domain consists of a list of authorized users, configuration settings, and an Amazon Elastic File System \(Amazon EFS\) volume, which contains data for the users, including notebooks, resources, and artifacts\. A user can have multiple applications \(apps\) which support the reading and execution experience of the user’s notebooks, terminals, and consoles\.

To return Studio to the state it was in before you onboarded, you must delete this domain\. The Amazon EFS volume isn't deleted when the domain is deleted\. To delete the EFS volume, see [Manage Your EFS Storage Volume](studio-tasks.md#studio-tasks-manage-storage)\.

You must delete the domain if you want to switch authentication modes from IAM to AWS SSO\.

To delete a domain, the domain cannot contain any user profiles\. To delete a user profile, the profile cannot contain any non\-failed apps\.

When you delete these resources, the following occurs:
+ App – The data \(files and notebooks\) in a user's home directory is saved\. Unsaved notebook data is lost\.
+ User profile – The user is no longer able to sign in to Studio and loses access to their home directory but the data is not deleted\. An admin can retrieve the data from the Amazon EFS volume where it is stored under the user's AWS account\.

**Note**  
You must have admin permission to delete a domain\.

You can only delete an app whose status is `InService`, which is displayed as **Ready** in Studio\. An app whose status is `Failed` doesn't need to be deleted to delete the containing domain\. In Studio, an attempt to delete an app in the failed state results in an error\.

**Topics**
+ [Delete a SageMaker Studio Domain \(Studio\)](#gs-studio-delete-domain-studio)
+ [Delete a SageMaker Studio Domain \(CLI\)](#gs-studio-delete-domain-cli)

## Delete a SageMaker Studio Domain \(Studio\)<a name="gs-studio-delete-domain-studio"></a>

**To delete a domain**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Amazon SageMaker Studio** at the top left of the page to open the **Amazon SageMaker Studio Control Panel**\.

1. Repeat the following steps for each user in the **User name** list\.

   1. Choose the user\.

   1. On the **User Details** page, for each non\-failed app in the **Apps** list, choose **Delete app**\.

   1. On the **Delete app** dialog, choose **Yes, delete app**, type *delete* in the confirmation field, and then choose **Delete**\.

   1. When the **Status** for all apps show as **Deleted**, choose **Delete user**\.
**Important**  
When a user is deleted, they lose access to the Amazon EFS volume that contains their data, including notebooks and other artifacts\.

1. When all users are deleted, choose **Delete Studio**\.

1. On the **Delete Studio** dialog, choose **Yes, delete Studio**, type *delete* in the confirmation field, and then choose **Delete**\.

## Delete a SageMaker Studio Domain \(CLI\)<a name="gs-studio-delete-domain-cli"></a>

For a list of AWS Regions supported by Amazon SageMaker Studio, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

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

1. Delete the domain\.

   ```
   aws --region Region sagemaker delete-domain \
       --domain-id DomainId
   ```