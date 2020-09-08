# Onboard to Amazon SageMaker Studio Using Quick Start<a name="onboard-quick-start"></a>

**Note**  
Amazon SageMaker Studio is available only in specific AWS Regions\. To view the list of supported Regions, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

This topic describes how to onboard to Amazon SageMaker Studio using the **Quick start** procedure, which uses AWS Identity and Access Management \(IAM\) authentication\. For information on how to onboard using the standard IAM procedure, see [Onboard Using IAM](onboard-iam.md)\.

For information on how to onboard using AWS Single Sign\-On \(AWS SSO\), see [Onboard Using SSO](onboard-sso-users.md)\.

**To onboard to Studio using **Quick start****

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Amazon SageMaker Studio** at the top left of the page\.

1. On the **Amazon SageMaker Studio** page, under **Get started**, choose **Quick start**\.

1. For **User name**, keep the default name or create a new name\. The name can be up to 63 characters\. Valid characters: A\-Z, a\-z, 0\-9, and \- \(hyphen\)\. 

1. For **Execution role**, choose an option from the role selector\.

   If you choose **Enter a custom IAM role ARN**, the role must have the AmazonSageMakerFullAccess policy attached\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:
   + For **S3 buckets you specify**, specify additional S3 buckets that users of your notebooks can access\. If you don't want to add access to more buckets, choose **None**\.
   + Choose **Create role**\. SageMaker creates a new IAM `AmazonSageMaker-ExecutionPolicy` role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. Choose **Submit**\.

   On the **Amazon SageMaker Studio Control Panel**, under **Studio Summary**, wait for **Status** to change to **Ready**\.

   When **Status** is **Ready**, the user name that you specified is enabled and chosen\. The **Add user** and **Delete user** buttons, and the **Open Studio** link are also enabled\.

1. Choose **Open Studio**\. The **Amazon SageMaker Studio** loading page displays\.

   When Studio opens you can start using it\.

Now that you've onboarded to SageMaker Studio, use the following steps to access Studio later\.

**To access Studio after you onboard**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Amazon SageMaker Studio** at the top left of the page\.

1. On the **Amazon SageMaker Studio Control Panel**, choose your user name and then choose **Open Studio**\.

**To add more users**

1. On the **Amazon SageMaker Studio Control Panel**, choose **Add user**\.

1. Repeat steps 4 and 5 from the first procedure, "To onboard to Studio using Quick start\."

1. Choose **Submit**\.

For information about using SageMaker Studio, see [Get Started with Studio](gs-studio.md)\.