# Add RStudio support to an existing Domain<a name="rstudio-add-existing"></a>

 If you have added an RStudio License through AWS License Manager, you can create a new Amazon SageMaker Domain with support for RStudio on SageMaker\. If you have an existing Domain that does not support RStudio, you can add RStudio support to that Domain without having to delete and recreate the Domain\.  

 The following topic outlines how to add this support\. 

## Prerequisites<a name="rstudio-add-existing-prerequisites"></a>

 You must complete the following steps before you update your current Domain to add support for RStudio on SageMaker\.  
+  Install and configure [AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) 
+  Configure the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config) with IAM credentials 
+  Create a Domain execution role following the steps in [Create a SageMaker Domain with RStudio using the AWS CLI](https://docs.aws.amazon.com/sagemaker/latest/dg/rstudio-create-cli.html#rstudio-create-cli-domainexecution)\. This Domain\-level IAM role is required by the RStudioServerPro app\. The role requires access to AWS License Manager for verifying a valid RStudio Workbench license and Amazon CloudWatch Logs for publishing server logs\.  
+  Bring your RStudio license to AWS License Manager following the steps in [RStudio license](https://docs.aws.amazon.com/sagemaker/latest/dg/rstudio-license.html)\. 
+  \(Optional\) If you want to use RStudio in `VPCOnly` mode, complete the steps in [RStudio in VPC\-Only](https://docs.aws.amazon.com/sagemaker/latest/dg/rstudio-network.html)\. 
+  Ensure that the security groups you have configured for each [UserProfile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html) in your Domain meet the account\-level quotas\. When configuring the default user profile during Domain creation, you can use the `DefaultUserSettings` parameter of the [CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html) API to add `SecurityGroups` that are inherited by all the user profiles created in the Domain\. You can also provide additional security groups for a specific user as part of the `UserSettings` parameter of the [CreateUserProfile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html) API\. If you have added security groups this way, you must ensure that the total number of security groups per user profile doesn’t exceed the maximum quota of 2 in `VPCOnly` mode and 4 in `PublicInternetOnly` mode\. If the resulting total number of security groups for any user profile exceeds the quota, you can combine multiple security groups’ rules into one security group\.  

## Add RStudio support to an existing Domain<a name="rstudio-add-existing-enable"></a>

After you have completed the prerequisites, you can add RStudio support to your existing Domain\. The following steps outline how to update your existing Domain to add support for RStudio\. 

### Step 1: Delete all apps in the Domain<a name="rstudio-add-existing-enable-step1"></a>

To add support for RStudio in your Domain, SageMaker must update the underlying security groups for all existing user profiles\. To complete this, you must delete and recreate all existing apps in the Domain\. The following procedure shows how to delete all of the apps\. 

1.  List all of the apps in the Domain\. 

   ```
   aws sagemaker \
      list-apps \
      --domain-id-equals <DOMAIN_ID>
   ```

1.  Delete each app for each user profile in the Domain\. 

   ```
   // JupyterServer apps 
   aws sagemaker \
       delete-app \
       --domain-id <DOMAIN_ID> \
       --user-profile-name <USER_PROFILE> \
       --app-type JupyterServer \
       --app-name <APP_NAME>
   
   // KernelGateway apps
   aws sagemaker \
       delete-app \
       --domain-id <DOMAIN_ID> \
       --user-profile-name <USER_PROFILE> \
       --app-type KernelGateway \
       --app-name <APP_NAME>
   ```

### Step 2 \- Update all user profiles with the new list of security groups<a name="rstudio-add-existing-enable-step2"></a>

 This is a one\-time action that you must complete for all of the existing user profiles in your Domain when you have refactored your existing security groups\. This prevents you from hitting the quota for the maximum number of security groups\. The `UpdateUserProfile` API call fails if the user has any apps that are in [InService](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeApp.html#sagemaker-DescribeApp-response-Status) status\. Delete all apps, then call `UpdateUserProfile` API to update the security groups\. 

**Note**  
The following requirement for `VPCOnly` mode outlined in [Connect Amazon SageMaker Studio Notebooks in a VPC to External Resources](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-notebooks-and-internet-access.html#studio-notebooks-and-internet-access-vpc) is no longer needed when adding RStudio support because `AppSecurityGroupManagement` is managed by the SageMaker service:  
“[TCP traffic within the security group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules-reference.html#sg-rules-other-instances)\. This is required for connectivity between the JupyterServer app and the KernelGateway apps\. You must allow access to at least ports in the range `8192-65535`\.” 

```
aws sagemaker \
    update-user-profile \
    --domain-id <DOMAIN_ID>\
    --user-profile-name <USER_PROFILE> \
    --user-settings "{\"SecurityGroups\": [\"<SECURITY_GROUP>\", \"<SECURITY_GROUP>\"]}"
```

### Step 3 \- Activate RStudio by calling the UpdateDomain API<a name="rstudio-add-existing-enable-step3"></a>

1.  Call the [UpdateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateDomain.html) API to add support for RStudio on SageMaker\. The `defaultusersettings` parameter is only needed if you have refactored the default security groups for your user profiles or would like to change the default RStudio access status of new user profiles created in your Domain\. 
   +  For `VPCOnly` mode: 

     ```
     aws sagemaker \
         update-domain \
         --domain-id <DOMAIN_ID> \
         --app-security-group-management Service \
         --domain-settings-for-update RStudioServerProDomainSettingsForUpdate={DomainExecutionRoleArn=<DOMAIN_EXECUTION_ROLE_ARN>} \
         --default-user-settings "{\"SecurityGroups\": [\"<SECURITY_GROUP>\", \"<SECURITY_GROUP>\"]}", "{\"RStudioServerProAppSettings\": {\"AccessStatus\": \"ENABLED\", \"userGroup\":\"R_STUDIO_USER\"}}"
     ```
   +  For `PublicInternetOnly` mode: 

     ```
     aws sagemaker \
         update-domain \
         --domain-id <DOMAIN_ID> \
         --domain-settings-for-update RStudioServerProDomainSettingsForUpdate={DomainExecutionRoleArn=<DOMAIN_EXECUTION_ROLE_ARN>} \
         --default-user-settings "{\"SecurityGroups\": [\"<SECURITY_GROUP>\", \"<SECURITY_GROUP>\"]}", "{\"RStudioServerProAppSettings\": {\"AccessStatus\": \"ENABLED\", \"userGroup\":\"R_STUDIO_USER\"}}"
     ```

1. Verify that the Domain status is `InService`\. After the Domain status is `InService`, support for RStudio on SageMaker is added\.

   ```
   aws sagemaker \
       describe-domain \
       --domain-id <DOMAIN_ID>
   ```

1. Verify that the RStudioServerPro app’s status is `InService` using the following command\.

   ```
   aws sagemaker list-apps --user-profile-name domain-shared
   ```

### Step 4 \- Add RStudio access for existing users<a name="rstudio-add-existing-enable-step4"></a>

 As part of the update in Step 3, SageMaker marks the RStudio [AccessStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RStudioServerProAppSettings.html#sagemaker-Type-RStudioServerProAppSettings-AccessStatus) of all existing user profiles in the Domain as `DISABLED` by default\. This prevents exceeding the number of users allowed by your current license\. To add access for existing users, there is a one\-time opt\-in step\. Perform the opt\-in by calling the [UpdateUserProfile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateUserProfile.html) API with the following [RStudioServerProAppSettings](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UserSettings.html#sagemaker-Type-UserSettings-RStudioServerProAppSettings): 
+  `AccessStatus` = `ENABLED` 
+  *Optional* \- `UserGroup` = `R_STUDIO_USER` or `R_STUDIO_ADMIN` 

```
aws sagemaker \
    update-user-profile \
    --domain-id <DOMAIN_ID>\
    --user-profile-name <USER_PROFILE> \
    --user-settings "{\"RStudioServerProAppSettings\": {\"AccessStatus\": \"ENABLED\"}}"
```

**Note**  
By default, the number of users that can have access to RStudio is 60\.

### Step 5 – Deactivate RStudio access for new users<a name="rstudio-add-existing-enable-step5"></a>

 Unless otherwise specified when calling `UpdateDomain`, RStudio support is added by default for all new user profiles created after you have added support for RStudio on SageMaker\. To deactivate access for a new user profile, you must explicitly set the `AccessStatus` parameter to `DISABLED` as part of the `CreateUserProfile` API call\. If the `AccessStatus` parameter is not specified as part of the `CreateUserProfile` API, the default access status is `ENABLED`\. 

```
aws sagemaker \
    create-user-profile \
    --domain-id <DOMAIN_ID>\
    --user-profile-name <USER_PROFILE> \
    --user-settings "{\"RStudioServerProAppSettings\": {\"AccessStatus\": \"DISABLED\"}}"
```