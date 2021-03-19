# Onboard to Amazon SageMaker Studio Using AWS SSO<a name="onboard-sso-users"></a>

This topic describes how to onboard to Amazon SageMaker Studio using AWS SSO authentication\. For information on how to onboard using AWS Identity and Access Management \(IAM\) authentication, see [Onboard Using Quick Start](onboard-quick-start.md) or [Onboard Using IAM](onboard-iam.md)\.

**To onboard to Studio using AWS SSO**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Amazon SageMaker Studio** at the top left of the page\.

1. On the **SageMaker Studio** page, under **Get started**, choose **Standard setup**\.

1. For **Authentication method**, choose **AWS Single Sign\-On \(SSO\)**\. A message tells you whether you have an AWS SSO account in an AWS Region supported by SageMaker Studio\.

1. If you don't have an AWS SSO account in a supported Region, you must create an AWS SSO account in a supported Region before proceeding\. To continue to onboard without creating a new AWS SSO account, choose the **AWS Identity and Access Management \(IAM\)** authentication method or the **Quick start** procedure, which also uses IAM\.

   For information about setting up AWS SSO for use with Studio, see [Set Up AWS SSO for Use with Amazon SageMaker Studio](onboard-sso-setup.md)\.

1. Under **Permission**, for **Execution role for all users**, choose an option from the role selector\.

   If you choose **Enter a custom IAM role ARN**, the role must have at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](sagemaker-roles.md)\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:

   1. For **S3 buckets you specify**, specify additional S3 buckets that users of your notebooks can access\. If you don't want to add access to more buckets, choose **None**\.

   1. Choose **Create role**\. SageMaker creates a new IAM `AmazonSageMaker-ExecutionPolicy` role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. For **Projects**, see [SageMaker Studio Permissions Required to Use Projects](sagemaker-projects-studio-updates.md)\.

1. Under **Network and storage**, specify the following:
   + Your VPC information – For more information, see [Choose a VPC](onboard-vpc.md)\.
   + \(Optional\) **Storage encryption key** – SageMaker uses an AWS managed customer master key \(CMK\) to encrypt your Amazon Elastic File System \(Amazon EFS\) and Amazon Elastic Block Store \(Amazon EBS\) file systems by default\. To use a customer managed CMK, enter its key ID or Amazon Resource Name \(ARN\)\. For more information, see [Protect Data at Rest Using Encryption](encryption-at-rest.md)\.

1. Choose **Assign users**\. The **Assign users** page opens and displays a list of your organization's members\.

1. To assign users access to SageMaker Studio, choose the check box next to their user name and choose **Assign users**\. 

1. Send each assigned user the **Studio address** link shown under **Studio Summary**\. Your AWS SSO users go to this address to access Studio\.

**To access Studio after onboarding**

After you are given access to Studio, you are sent an email inviting you to create a password and activate your AWS SSO account\. The email also contains the URL to sign in to Studio\.

After you activate your account, go to the Studio URL, sign in, and wait for your user profile to be created\. On subsequent visits, you only need to wait for Studio to load\.

Bookmark the Studio URL\. The URL is also available in the Studio Control Panel\.

For information about using SageMaker Studio, see [SageMaker Studio](studio.md)\.