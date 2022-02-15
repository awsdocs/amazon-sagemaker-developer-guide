# Manage users<a name="rstudio-create-user"></a>

After your RStudio\-enabled Amazon SageMaker Domain is running, you can add user profiles \(UserProfiles\) to the Domain\. The following topics show how to create user profiles that are authorized to use RStudio, as well as update an existing user profile\. For information on how to delete an RStudio App, UserProfile, or Domain, follow the steps in [Delete an Amazon SageMaker Domain](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html)\. 

**Note**  
The limit for the total number of UserProfiles in a Amazon SageMaker Domain is 60\.

 There are two types of users: 
+ Unauthorized: This user cannot access the RStudio app\. 
+ Authorized: This user can access the RStudio app and use one of the RStudio license seats\. By default, a new user is `Authorized` if the Domain is enabled for RStudio\.

If a user is authorized, they can be given one of the following levels of access to RStudio\. 
+  RStudio User: This is a standard RStudio user and can access RStudio\. 
+  RStudio Admin: The admin of your Amazon SageMaker Domain has the ability to create users, add existing users, and update the permissions of existing users\. Admins can also access the RStudio Administrative dashboard\. However, this admin is not able to update parameters that are managed by Amazon SageMaker\.

## Methods to create a user<a name="rstudio-create-user-methods"></a>

The following topics show how to create a user in your RStudio\-enabled Amazon SageMaker Domain\.

 **Create user IAM** 

The following procedure shows how to add users to a Amazon SageMaker Domain created using IAM\. For more information about using IAM with Amazon SageMaker, see [How Amazon SageMaker Works with IAM ](https://docs.aws.amazon.com/sagemaker/latest/dg/security_iam_service-with-iam.html)\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  Navigate to the **Amazon SageMaker Domain Control Panel**\.

1. Select **Add user**\. This opens a new **User Settings** page\.

1.  Under **User profile**, enter a name for your user and select an IAM role\. You can create a new IAM role or use an existing role\. The IAM role must have the `AmazonSageMakerFullAccess` policy attached\.

1.  Select **Next**\.

1.  Under **SageMaker Projects and Jumpstart**, select whether to enable Amazon SageMaker project templates and Amazon SageMaker JumpStart for Studio users\.

1.  Select **Next**\.

1.  Under **RStudio Workbench**, verify that an RStudio Workbench license is detected\.

1. Under **License Authorization**, select whether you want to create the user with one of the following authorizations\.
   +  Unauthorized 
   +  RStudio Admin 
   +  RStudio User 

1. Select **Submit**\.

 **Create user SSO** 

The following procedure shows how to add users to a Amazon SageMaker Domain created using AWS Single Sign\-On\. For information about AWS Single Sign\-On, see [What is AWS Single Sign\-On?](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html)\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  Navigate to the **Amazon SageMaker Domain Control Panel**\.

1.  Select **Assign users and groups**\. This opens a new Assign users and groups page\. 

1.  Select a user or group from the list\. For information about adding users and groups, see [Manage identities in AWSAWS SSO](https://docs.aws.amazon.com/singlesignon/latest/userguide/manage-your-identity-source-sso.html)\. 

1.  Select **Assign users and groups**\. 

 **Create user CLI** 

 The following command shows how to add users to a Amazon SageMaker Domain with IAM authentication\. A User can belong to either the `R_STUDIO_USER` or `R_STUDIO_ADMIN` User group\. 

```
aws sagemaker create-user-profile --region <REGION> \
    --domain-id <DOMAIN-ID> \
    --user-profile-name <USER_PROFILE_NAME-ID> \
    --user-settings RStudioServerProAppSettings={UserGroup=<USER-GROUP>}
```

The following command shows how to add users to a Amazon SageMaker Domain with AWS SSO authentication\. A user can belong to either the `R_STUDIO_USER` or `R_STUDIO_ADMIN` User group\. 

```
aws sagemaker create-user-profile --region <REGION> \
    --domain-id <DOMAIN-ID> \
    --user-profile-name <USER_PROFILE_NAME-ID> \
    --user-settings RStudioServerProAppSettings={UserGroup=<USER-GROUP>} \
    --single-sign-on-user-identifier UserName \
    --single-sign-on-user-value <USER-NAME>
```

## Update existing user<a name="rstudio-create-user-update"></a>

You cannot update the authorization of an existing user\. You must delete the existing user and create a new one with the updated authorization\.

 **Log in to RStudio as another user** 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  Navigate to the **Amazon SageMaker Domain Control Panel**\.

1.  Select a user name from the list of users\. This opens a new page with details about the user profile and the apps that are running\. 

1.  Select the **Launch App**\. 

1.  From the dropdown, select **RStudio** to launch an RStudio instance\. 

 **Terminate sessions for another user** 

1.  From the list of running apps, identify the app you want to delete\. 

1.  Click the respective **Delete app** button for the app you are deleting\. 

 **Delete another user** 

 You cannot delete a user if the user is running any apps\. Delete all apps before attempting to delete a user\. 

1.  From the **User Profile** page, select **Edit**\. This opens a new **General settings** page\. 

1.  Under **Delete user**, select **Delete user**\. 