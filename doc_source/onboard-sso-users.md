# Onboard to Amazon SageMaker Domain Using AWS SSO<a name="onboard-sso-users"></a>

This topic describes how to onboard to Amazon SageMaker Domain using AWS SSO authentication\. For information on how to onboard using AWS Identity and Access Management \(IAM\) authentication, see [Onboard Using Quick Start](onboard-quick-start.md) or [Onboard Using IAM](onboard-iam.md)\.

**To onboard to Domain using AWS SSO**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **SageMaker Domain** at the top left of the page\.

1. On the **SageMaker Domain** page, under **Choose setup method**, choose **Standard setup**\.

1. Select **Configure**\.

**Step 1: General settings**

1. For **Authentication**, choose **AWS Single Sign\-On \(SSO\)**\.

1. If you don't have an AWS SSO account in the same Region as your SageMaker Domain, you must create an AWS SSO account in the same Region as your SageMaker Domain before proceeding\. To continue to onboard without creating a new AWS SSO account, choose the **AWS Identity and Access Management \(IAM\)** authentication method or the **Quick start** procedure, which also uses IAM\.

   For information about setting up AWS SSO for use with Domain, see [Set Up AWS SSO for Use with Amazon SageMaker Domain](onboard-sso-setup.md)\.
**Note**  
To associate a SageMaker Studio UserProfile with AWS SSO, the UserProfile must be have been created using the AWS console\.

1. Under **Permission**, for **IAM role**, choose an option from the role selector\.

   If you choose **Enter a custom IAM role ARN**, the role must have at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](sagemaker-roles.md)\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:

   1. For **S3 buckets you specify**, specify additional S3 buckets that users of your notebooks can access\. If you don't want to add access to more buckets, choose **None**\.

   1. Choose **Create role**\. SageMaker creates a new IAM `AmazonSageMaker-ExecutionPolicy` role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. Under **Network and storage**, specify the following:
   + Your VPC information – For more information, see [Choose a VPC](onboard-vpc.md)\.
   + \(Optional\) **Encryption key** – SageMaker uses an AWS KMS key to encrypt your Amazon Elastic File System \(Amazon EFS\) and Amazon Elastic Block Store \(Amazon EBS\) file systems\. By default, it uses an AWS managed key\. To use a customer managed key, enter its key ID or Amazon Resource Name \(ARN\)\. For more information, see [Protect Data at Rest Using Encryption](encryption-at-rest.md)\.
**Note**  
Encryption in transit is only available for Amazon SageMaker Studio\.

1. Select **Next**\. 

**Step 2: Studio settings**

1. Under **Notebook Sharing Configuration**, accept the default notebook sharing configuration or customize the options\.

1. Under **SageMaker Projects and JumpStart**, accept the default Project and JumpStart settings, or customize whether administrators and users can create projects and use Jumpstart\. For more information, see [SageMaker Studio Permissions Required to Use Projects](sagemaker-projects-studio-updates.md)\.

1. Select **Next**\. 

**Step 3: RStudio settings**

1. Under **RStudio Workbench**, verify that your RStudio license is automatically detected\. For more information about getting an RStudio license and activating it with SageMaker, see [RStudio license](rstudio-license.md)\.

1. Select an instance type to launch your RStudio Server on\. For more information, see [RStudioServerPro instance type](rstudio-select-instance.md)\.

1. Under **Permission**, create your role or select an existing role\. The role must have the following permissions policy\. This policy allows the RStudioServerPro app to access necessary resources and allows Amazon SageMaker to automatically launch an RStudioServerPro app when the existing RStudioServerPro app is in a `Deleted` or `Failed` status\. For information about adding permissions to a role, see [Modifying a role permissions policy \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_permissions-policy)\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "license-manager:ExtendLicenseConsumption",
                   "license-manager:ListReceivedLicenses",
                   "license-manager:GetLicense",
                   "license-manager:CheckoutLicense",
                   "license-manager:CheckInLicense",
                   "logs:CreateLogDelivery",
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:DeleteLogDelivery",
                   "logs:Describe*",
                   "logs:GetLogDelivery",
                   "logs:GetLogEvents",
                   "logs:ListLogDeliveries",
                   "logs:PutLogEvents",
                   "logs:PutResourcePolicy",
                   "logs:UpdateLogDelivery",
                   "sagemaker:CreateApp"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Under **RStudio Connect**, add the URL for your RStudio Connect server\. RStudio Connect is a publishing platform for Shiny applications, R Markdown reports, dashboards, plots, and more\. When you onboard to RStudio on SageMaker, an RStudio Connect server is not created\. For more information, see [RStudio Connect URL](rstudio-configure-connect.md)\.

1. Under **RStudio Package Manager**, add the URL for your RStudio Package Manager\. SageMaker creates a default package repository for the Package Manager when you onboard RStudio\. For more information about RStudio Package Manager, see [RStudio Package Manager](rstudio-configure-pm.md)\. 

1. Select **Submit**\. 

**To access the Domain after onboarding**

After you are given access to the Domain, you are sent an email inviting you to create a password and activate your AWS SSO account\. The email also contains the URL to sign in to the Domain\. For more information about signing in and session duration, see [How to sign in to the user portal](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtosignin.html)\.

After you activate your account, go to the Domain URL, sign in, and wait for your user profile to be created\. On subsequent visits, you only need to wait for the Studio or RStudio app to load\.

Bookmark the URL\. The URL is also available in the **SageMaker Domain** Control Panel\.

For information about using SageMaker Studio, see [SageMaker Studio](studio.md)\.

For information about using RStudio, see [RStudio on Amazon SageMaker](rstudio.md)\.