# Onboard to Amazon SageMaker Domain Using Quick setup<a name="onboard-quick-start"></a>

This topic describes how to onboard to Amazon SageMaker Domain using the **Quick setup** procedure, which uses AWS Identity and Access Management \(IAM\) authentication\. For information on how to onboard using the standard IAM procedure, see [Onboard Using IAM](onboard-iam.md)\.

RStudio support is not currently available when onboarding using the **Quick setup** procedure\.

For information on how to onboard using AWS Single Sign\-On \(AWS SSO\), see [Onboard Using SSO](onboard-sso-users.md)\.

**To onboard to the Domain using **Quick setup****

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Control Panel** at the top left of the page\.

1. On the **Setup SageMaker Domain** page, choose **Quick setup**\.

1. Under **User profile**, for **Name** keep the default name or create a new name\. The name can be up to 63 characters\. Valid characters: A\-Z, a\-z, 0\-9, and \- \(hyphen\)\. 

1. For **Default execution role**, choose an option from the role selector\. This is the default role that is assigned to the **Amazon SageMaker Domain** user profile\.

   If you choose **Enter a custom IAM role ARN**, the role must have at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](sagemaker-roles.md)\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:
   + For **S3 buckets you specify**, specify additional S3 buckets that users of your notebooks can access\. If you don't want to add access to more buckets, choose **None**\.
   + Choose **Create role**\. SageMaker creates a new IAM `AmazonSageMaker-ExecutionPolicy` role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. Choose **Submit**\.

1. From the pop\-up window, select a Amazon Virtual Private Cloud \(Amazon VPC\) and subnet to use\.

1. Choose **Save and continue**\.
**Note**  
If you receive an error message that you need to create a VPC, see [Choose a VPC](onboard-vpc.md)\.

   When **Status** is **Ready**, the user name that you specified is enabled and chosen\. The **Add user** and **Delete user** buttons, and the **Launch app** link are also enabled\.

1. From **Launch app**, select **Studio**\. The **Amazon SageMaker Studio** loading page displays\.

   When Studio opens, you can start using it\.

Now that you've onboarded to SageMaker Studio, use the following steps to access Studio later\.

**To access Studio after you onboard**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Control Panel** at the top left of the page\.

1. On the **Control Panel**, choose your user name and then choose **Launch app**\. Select **Studio**\.

**To add more users**

1. On the **Control Panel**, choose **Add user**\.

1. Repeat steps 4 and 5 from the first procedure, "To onboard to Amazon SageMaker Domain using Quick setup\."

1. Choose **Submit**\.

For information about using SageMaker Studio, see [SageMaker Studio](studio.md)\.