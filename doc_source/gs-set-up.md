# Set Up Amazon SageMaker<a name="gs-set-up"></a>

In this section, you sign up for an AWS account, create an IAM admin user, and onboard to Amazon SageMaker Studio\.

If you're new to SageMaker, we recommend that you read [How Amazon SageMaker Works](whatis.md#how-it-works)\.

**Topics**
+ [Create an AWS Account](#gs-account)
+ [Create an IAM Administrator User and Group](#gs-account-user)

## Create an AWS Account<a name="gs-account"></a>

In this section, you sign up for an AWS account\. If you already have an AWS account, skip this step\.

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all AWS services, including SageMaker\. You are charged only for the services that you use\. 

**To create an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

Write down your AWS account ID because you'll need it for the next task\.

## Create an IAM Administrator User and Group<a name="gs-account-user"></a>

When you create an AWS account, you get a single sign\-in identity that has complete access to all of the AWS services and resources in the account\. This identity is called the AWS account *root user*\. Signing in to the AWS console using the email address and password that you used to create the account gives you complete access to all of the AWS resources in your account\. 

We strongly recommend that you *not* use the root user for everyday tasks, even the administrative ones\. Instead, adhere to the [Create Individual IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users), an AWS Identity and Access Management \(IAM\) administrator user\. Then securely lock away the root user credentials and use them to perform only a few account and service management tasks\. 

**To create an administrator user**
+ Create an administrator user in your AWS account\. For instructions, see [Creating Your First IAM User and Administrators Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
**Note**  
We assume that you use administrator user credentials for the exercises and procedures in this guide\. If you choose to create and use another IAM user, grant that user minimum permissions\. For more information, see [Authenticating with Identities](security-iam.md#security_iam_authentication)\.