# Set Up Amazon SageMaker Prerequisites<a name="gs-set-up"></a>

In this section, you sign up for an AWS account and create an AWS Identity and Access Management \(IAM\) admin user\.

If you're new to SageMaker, we recommend that you read [How Amazon SageMaker Works](whatis.md#how-it-works)\.

**Topics**
+ [Create an AWS Account](#gs-account)
+ [Create an Administrative User and Group](#gs-account-user)
+ [AWS CLI Prerequisites](#gs-cli-prereq)

## Create an AWS Account<a name="gs-account"></a>

In this section, you sign up for an AWS account\. If you already have an AWS account, skip this step\.

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all AWS services, including SageMaker\. You are charged only for the services that you use\. 

**To create an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

Write down your AWS account ID because you'll need it for the next task\.

## Create an Administrative User and Group<a name="gs-account-user"></a>

When you create an AWS account, you get a single sign\-in identity that has complete access to all of the AWS services and resources in the account\. This identity is called the AWS account *root user*\. Signing in to the AWS console using the email address and password that you used to create the account gives you complete access to all of the AWS resources in your account\. 

We strongly recommend that you *not* use the root user for everyday tasks, even the administrative ones\. Instead, adhere to the [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html), and create an administrative user\. Then securely lock away the root user credentials and use them to perform only a few account and service management tasks\.

**To create an administrative user**

1. Create an administrative user in your AWS account\. For instructions, see [Create an administrative user](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
**Note**  
We assume that you use administrator user credentials for the exercises and procedures in this guide\. If you choose to create and use another user, grant that user minimum permissions\. For more information, see [Authenticating with Identities](security-iam.md#security_iam_authentication)\.

1. Ensure that your administrator user has the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy, as well as a policy with the following content needed to create a SageMaker domain\. For more information about creating IAM policies, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html)\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "sagemaker:*"
               ],
               "Resource": [
                   "arn:aws:sagemaker:*:*:domain/*",
                   "arn:aws:sagemaker:*:*:user-profile/*",
                   "arn:aws:sagemaker:*:*:app/*",
                   "arn:aws:sagemaker:*:*:flow-definition/*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "iam:GetRole",
                   "servicecatalog:*"
               ],
               "Resource": [
                   "*"
               ]
           }
       ]
   }
   ```

## AWS CLI Prerequisites<a name="gs-cli-prereq"></a>

The following prerequisites are required to onboard to a Domain using the AWS CLI\.
+  Update the AWS CLI by following the steps in [Installing the current AWS CLI Version](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html#install-tool-bundled)\. 
+  From your local machine, run `aws configure` and provide your AWS credentials\. For information about AWS credentials, see [Understanding and getting your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\. 