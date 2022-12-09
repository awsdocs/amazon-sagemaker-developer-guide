# Getting Started with Amazon SageMaker geospatial capabilities<a name="geospatial-getting-started"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

This guide demonstrates how to complete the necessary steps to satisfy the prerequisites for using SageMaker geospatial capabilities\.

## Step 1: Create an AWS account<a name="geospatial-setup"></a>

To use SageMaker geospatial you need to have an AWS account\. If you already have an AWS account, skip this step\.

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all AWS services, including SageMaker\.

**To create an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

1. Write down your AWS account ID, because you need it for the next task\.

1. AWS sends you a confirmation email after the sign up process is complete\. At any time, you can view your current account activity and manage your account by going to [https://aws\.amazon\.com/](https://aws.amazon.com/) and choosing **My Account**\.

## Step 2: Setup the SageMaker geospatial UI and SageMaker Studio notebook with a SageMaker geospatial image<a name="geospatial-sdk-console-setup"></a>

See [Onboard to Amazon SageMaker Domain](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html), which provides you with steps to create a Domain, giving you access to Amazon SageMaker Studio and Amazon SageMaker geospatial capabilities\.

**To use the SageMaker geospatial UI:**

Within the Studio UI, choose **Geospatial** under **Data** from the left navigation panel on the **Home** menu\.

**To use a SageMaker Studio notebook with a SageMaker geospatial image:**

1. From the **Launcher**, choose **Create Notebook**\.

1. Next, the **Set up environment** dialog opens\.

1. Select the **Image** dropdown and choose **Geospatial 1\.0**\. The **Instance type** should be **ml\.m5\.4xlarge**\. Do not change the default values for other settings\.

1. Choose **Select**\.