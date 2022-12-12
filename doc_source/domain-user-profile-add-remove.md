# Add and Remove User Profiles<a name="domain-user-profile-add-remove"></a>

 The following sections demonstrate how to add and remove user profiles from an Amazon SageMaker Domain using the SageMaker console or the AWS Command Line Interface \(AWS CLI\)\. 

**Topics**
+ [Add user profiles](#domain-user-profile-add)
+ [Remove user profiles](#domain-user-profile-remove)

## Add user profiles<a name="domain-user-profile-add"></a>

 The following section shows how to add user profiles to a Domain using the SageMaker console or the AWS CLI\. 

### Add user profiles from the console<a name="domain-user-profile-add-console"></a>

 You can add user profiles to a Domain from the SageMaker console by following this procedure\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation pane, choose **Domains**\.

1. From the list of Domains, select the Domain that you want to add a user profile to\.

1. On the **Domain details** page, choose the **User profiles** tab\.

1. Choose **Add user**\. This opens a new page\.

1. Use the default name for your user profile or add a custom name\.

1.  For **Execution role**, choose an option from the role selector\. If you choose **Enter a custom IAM role ARN**, the role must have, at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\. 

   If you choose **Create a new role**, the **Create an IAM role** dialog box opens:

   1.  For **S3 buckets you specify**, specify additional Amazon S3 buckets that users of your notebooks can access\. If you don't want to add access to more buckets, choose **None**\. 

   1.  Choose **Create role**\. SageMaker creates a new IAM role, `AmazonSageMaker-ExecutionPolicy`, with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\. 

1. \(Optional\) Add tags to the user profile\. All resources that the user profile creates will have a Domain ARN tag and a user profile ARN tag\. The Domain ARN tag is based on Domain ID, while the user profile ARN tag is based on the user profile name\.

1. Choose **Next**\.

1.  Under **Default JupyterLab version**, select a JupyterLab version from the dropdown to use as the default for your user profile\. For information about selecting a JupyterLab version, see [JupyterLab Versioning](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jl.html)\.

1. In the **SageMaker Projects and JumpStart** section, you have two options\. You can accept the default Project and JumpStart settings, or you can customize whether the user profile can create projects and use JumpStart\. For more information, see [SageMaker Studio Permissions Required to Use Projects](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-projects-studio-updates.html)\.

1. Choose **Next**\.

1. \(Optional\) If the Domain has an RStudio license associated, select whether you want to create the user with one of the following authorizations:
   +  Unauthorized 
   +  RStudio Admin 
   +  RStudio User 

1. Choose **Next**\.

1. For the **Canvas base permissions configuration**, select whether to establish the minimum required permissions to use the SageMaker Canvas application\.

1. \(Optional\) For the **Time series forecasting configuration**: To grant user permissions for time series forecasting in SageMaker Canvas, leave the **Enable time series forecasting** option turned on\. It is turned on by default\.

1. \(Optional\) If you left **Enable time series forecasting** turned on, select **Create and use a new execution role**\. Alternatively, if you already have an IAM role with the required Amazon Forecast permissions attached, select **Use an existing execution role**\. For more information, see the [IAM role setup method](canvas-set-up-forecast.md#canvas-set-up-forecast-iam)\.

1. Choose **Submit**\.

### Create user profiles from the AWS CLI<a name="domain-user-profile-add-cli"></a>

To create a user profile in a Domain from the AWS CLI, run the following command from the terminal of your local machine\. For information about the available JupyterLab version ARNs, see [Setting a default JupyterLab version](studio-jl.md#studio-jl-set)\.

```
aws --region region \
sagemaker create-user-profile \
--domain-id domain-id \
--user-profile-name user-name \
--user-settings '{
  "JupyterServerAppSettings": {
    "DefaultResourceSpec": {
      "SageMakerImageArn": "sagemaker-image-arn",
      "InstanceType": "system"
    }
  }
}'
```

## Remove user profiles<a name="domain-user-profile-remove"></a>

 All apps launched by a user profile must be deleted to delete the user profile\. The following section shows how to remove user profiles from a Domain using the SageMaker console or AWS CLI\. 

### Remove user profiles from the console<a name="domain-user-profile-remove-console"></a>

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation pane, choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to remove a user profile from\. 

1. On the **Domain details** page, choose the **User profiles** tab\. 

1.  Select the user profile that you want to delete\. The user profile must not contain any non\-failed apps\. 

1. On the **User details** page, choose **Edit**\. 

1.  Choose **Delete user**\. This opens a new pop\-up\. 

1. On the **Delete user** pop\-up, choose **Yes, delete user**\. 

1. Enter *delete* in the field to confirm deletion\. 

1.  Choose **Delete**\. 

### Remove user profiles from the AWS CLI<a name="domain-user-profile-remove-cli"></a>

To delete a user profile from the AWS CLI, run the following command from the terminal of your local machine\.

```
aws sagemaker delete-user-profile \
--region region \
--domain-id domain-id \
--user-profile-name user-name
```