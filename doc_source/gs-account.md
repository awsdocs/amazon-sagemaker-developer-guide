# Step 1\.1: Create an AWS Account and an Administrator User<a name="gs-account"></a>

Before you use Amazon SageMaker for the first time, complete the following tasks: 

**Topics**
+ [Step 1\.1\.1: Create an AWS Account](#gs-account-create)
+ [Step 1\.1\.2: Create an IAM Administrator User and Sign In](#gs-account-user)

## Step 1\.1\.1: Create an AWS Account<a name="gs-account-create"></a>

If you already have an AWS account, skip this step\.

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all AWS services, including Amazon SageMaker\. You are charged only for the services that you use\. 

**To create an AWS account**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
If you previously signed in to the AWS Management Console using AWS account root user credentials, choose **Sign in to a different account**\. If you previously signed in to the console using IAM credentials, choose **Sign\-in using root account credentials**\. Then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code using the phone keypad\.

Write down your AWS account ID because you'll need it for the next task\.

## Step 1\.1\.2: Create an IAM Administrator User and Sign In<a name="gs-account-user"></a>

When you create an AWS account, you get a single sign\-in identity that has complete access to all of the AWS services and resources in the account\. This identity is called the AWS account *root user*\. Signing in to the AWS console using the email address and password that you used to create the account gives you complete access to all of the AWS resources in your account\. 

We strongly recommend that you *not* use the root user for everyday tasks, even the administrative ones\. Instead, adhere to the [Create Individual IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users), an AWS Identity and Access Management \(IAM\) administrator user\. Then securely lock away the root user credentials and use them to perform only a few account and service management tasks\. 

**To create an administrator user and sign in to the console**

1. Create an administrator user in your AWS account\. For instructions, see [Creating Your First IAM User and Administrators Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
**Note**  
We assume that you use administrator user credentials for the exercises and procedures in this guide\. If you choose to create and use another IAM user, grant that user minimum permissions\. For more information, see [Authentication and Access Control for Amazon SageMaker](authentication-and-access-control.md)\.

1. Sign in to the AWS Management Console\. 

   To sign in to the AWS console as a IAM user, you must use a special URL\. For more information, see [How Users Sign In to Your Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_how-users-sign-in.html) in the *IAM User Guide*\.

**Next Step**  
[Step 1\.2: Create an S3 Bucket](gs-config-permissions.md)