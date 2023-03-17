# Getting Started with Amazon SageMaker geospatial capabilities<a name="geospatial-getting-started"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

This guide demonstrates how to complete the necessary steps to satisfy the prerequisites for using SageMaker geospatial capabilities\.

To use SageMaker geospatial capabilities you need to have an AWS account\. If you already have an AWS account, skip this step\.

## Sign up for an AWS account<a name="sign-up-for-aws"></a>

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

AWS sends you a confirmation email after the sign\-up process is complete\. At any time, you can view your current account activity and manage your account by going to [https://aws\.amazon\.com/](https://aws.amazon.com/) and choosing **My Account**\.

## Create an administrative user<a name="create-an-admin"></a>

After you sign up for an AWS account, create an administrative user so that you don't use the root user for everyday tasks\.

**Secure your AWS account root user**

1.  Sign in to the [AWS Management Console](https://console.aws.amazon.com/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.

   For help signing in by using root user, see [Signing in as the root user](https://docs.aws.amazon.com/signin/latest/userguide/console-sign-in-tutorials.html#introduction-to-root-user-sign-in-tutorial) in the *AWS Sign\-In User Guide*\.

1. Turn on multi\-factor authentication \(MFA\) for your root user\.

   For instructions, see [Enable a virtual MFA device for your AWS account root user \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html#enable-virt-mfa-for-root) in the *IAM User Guide*\.

**Create an administrative user**
+ For your daily administrative tasks, grant administrative access to an administrative user in AWS IAM Identity Center \(successor to AWS Single Sign\-On\)\.

  For instructions, see [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.

**Sign in as the administrative user**
+ To sign in with your IAM Identity Center user, use the sign\-in URL that was sent to your email address when you created the IAM Identity Center user\.

  For help signing in using an IAM Identity Center user, see [Signing in to the AWS access portal](https://docs.aws.amazon.com/signin/latest/userguide/iam-id-center-sign-in-tutorial.html) in the *AWS Sign\-In User Guide*\.

## Create an execution role and trust policy<a name="geospatial-role-setup"></a>

As a managed service, Amazon SageMaker geospatial capabilities perform operations on your behalf on the AWS hardware that is managed by SageMaker\. It can perform only operations that the user permits\. To work with SageMaker geospatial capabilities you need to setup a user role and an execution role\. See [Amazon SageMaker geospatial capabilities roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-geospatial-roles.html) to learn more\.

## Setup the SageMaker geospatial UI<a name="geospatial-sdk-console-setup"></a>

See [Onboard to Amazon SageMaker Domain](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html), which provides you with steps to create a Domain, giving you access to Amazon SageMaker Studio and Amazon SageMaker geospatial capabilities\.

**To use the SageMaker geospatial UI:**

**Note**  
Currently, SageMaker geospatial capabilities are only supported in the US West \(Oregon\) Region for public preview\. To view Amazon SageMaker geospatial capabilities, choose the name of the currently displayed Region in the navigation bar of the console\. Then choose the US West \(Oregon\) Region\.

Within the Studio UI, choose **Geospatial** under **Data** from the left navigation panel on the **Home** menu\.