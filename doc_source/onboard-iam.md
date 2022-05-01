# Onboard to Amazon SageMaker Domain Using IAM<a name="onboard-iam"></a>

This topic describes how to onboard to Amazon SageMaker Domain using the standard setup procedure for AWS Identity and Access Management \(IAM\) authentication\. To onboard faster using IAM, see [Onboard Using Quick setup](onboard-quick-start.md)\.

For information on how to onboard using AWS Single Sign\-On \(AWS SSO\), see [Onboard Using SSO](onboard-sso-users.md)\.

**To onboard to Domain using IAM**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Control Panel** at the top left of the page\.

1. On the **Setup SageMaker Domain** page, choose **Standard setup**\.

1. Select **Configure**\.

**Step 1: General settings**

1. For **Authentication**, choose **AWS Identity and Access Management \(IAM\)**\.

1. Under **Permission**, for **IAM role**, choose an option from the role selector\.

   If you choose **Enter a custom IAM role ARN**, the role must have at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](sagemaker-roles.md)\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:

   1. For **S3 buckets you specify**, specify additional S3 buckets that users of your notebooks can access\. If you don't want to add access to more buckets, choose **None**\.

   1. Choose **Create role**\. SageMaker creates a new IAM `AmazonSageMaker-ExecutionPolicy` role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. Under **Network and storage**, specify the following:
   + Your VPC information – For more information, see [Choose a VPC](onboard-vpc.md)\.
   + \(Optional\) **Storage encryption key** – SageMaker uses an AWS KMS key to encrypt your Amazon Elastic File System \(Amazon EFS\) and Amazon Elastic Block Store \(Amazon EBS\) file systems\. By default, it uses an AWS managed key\. To use a customer managed key, enter its key ID or Amazon Resource Name \(ARN\)\. For more information, see [Protect Data at Rest Using Encryption](encryption-at-rest.md)\.
**Note**  
Encryption in transit is only available for Amazon SageMaker Studio\.

1. Select **Next**\. 

**Step 2: Studio settings**

1. Under **Notebook Sharing Configuration**, accept the default notebook sharing configuration or customize the options\.

1. Under **SageMaker Projects and JumpStart**, accept the default Project and JumpStart settings or customize whether administrators and user can create projects and use Jumpstart\. For more information, see [SageMaker Studio Permissions Required to Use Projects](sagemaker-projects-studio-updates.md)\.

1. Select **Next**\. 

**Step 3: RStudio settings**

1. Under **RStudio Workbench**, verify that your RStudio license is automatically detected\. For more information about getting an RStudio license and activating it with SageMaker, see [RStudio license](rstudio-license.md)\.

1. Select an instance type to launch your RStudio Server on\. For more information, see [RStudioServerPro instance type](rstudio-select-instance.md)\.

1. Under **Permission**, create your role or select an existing role\. The role must have the following permissions policy\. This policy allows the RStudioServerPro app to access necessary resources and allows Amazon SageMaker to automatically launch an RStudioServerPro app when the existing RStudioServerPro app is in a `Deleted` or `Failed` status\. For information on adding permissions to a role, see [Modifying a role permissions policy \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_permissions-policy)\.

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

1. Under **RStudio Connect**, add the URL for your RStudio Connect Server\. RStudio Connect is a publishing platform for Shiny applications, R Markdown reports, dashboards, plots, and more\. When you onboard to RStudio on Amazon SageMaker, an RStudio Connect server is not created\. You must create an RStudio Connect server on an EC2 instance to use Connect with Amazon SageMaker\. For more information, see [RStudio Connect URL](rstudio-configure-connect.md)\.

1. Under **RStudio Package Manager**, add the URL for your RStudio Package Manager\. SageMaker creates a default package repository for the Package Manager when you onboard RStudio\. For more information about RStudio Package Manager, see [RStudio Package Manager](rstudio-configure-pm.md)\. 

1. Select **Submit**\. 

Now that you've onboarded to the **SageMaker Domain**, use the following steps to subsequently access Studio or RStudio\.

**Access **SageMaker Domain** after you onboard**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Control Panel** at the top left of the page\.

1. On the **Control Panel**, choose your user name and then choose **Launch app**\. Select either Studio or RStudio\.

For information about using SageMaker Studio, see [SageMaker Studio](studio.md)\.

For information about using RStudio, see [RStudio on Amazon SageMaker](rstudio.md)\.