# Onboard to Amazon SageMaker Studio Using IAM<a name="onboard-iam"></a>


****  

|  | 
| --- |
| Amazon SageMaker Studio Notebooks is in preview release and is subject to change\. | 

**Note**  
Amazon SageMaker Studio is available only in specific AWS Regions\. To view the list of supported Regions, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

This procedure describes how to onboard to Amazon SageMaker using the standard setup process for AWS Identity and Access Management \(IAM\) authentication\. For a procedure to onboard faster using IAM, see [Onboard Using Quick Start](onboard-quick-start.md)\. For information about onboarding using AWS SSO authentication, see [Onboard Using SSO](onboard-sso-users.md)\.

**To onboard to Studio using IAM**

1. Open the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/)\. 

1. Choose **Amazon SageMaker Studio** at the top left of the page\.

1. On the **Amazon SageMaker Studio Control Panel**, under **Get started**, choose **Standard setup**\.

1. For **Authentication method**, choose **AWS Identity and Access Management \(IAM\)**\.

1. Under **Permission**, for **Execution role for all users**, choose an option from the role selector\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:
   + For **S3 buckets you specify**, specify additional S3 buckets that users of your notebooks can access\. If you don't want to add access to more buckets, choose **None**\.
   + Choose **Create role**\. Amazon SageMaker creates a new IAM role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. Choose **Submit**\.

   On the **Amazon SageMaker Studio Control Panel**, under **Studio Summary**, wait for **Status** to change to **Ready** and the **Add user** button to be enabled\.

1. Choose **Add user**\.

1. On the **Add user** page, keep the default name or create a new name\. A name can be up to 63 characters\. Valid characters: A\-Z, a\-z, 0\-9, and \- \(hyphen\)\. 

   For **Execution role**, choose an option from the role selector\.

1. Choose **Submit**\. The **Amazon SageMaker Studio Control Panel** opens with the new user listed\. The **Delete user** button and the **Open Studio** link are both enabled\.

1. To add more users, repeat steps 7 through 9\.

   When multiple users are listed, none of the users in the user list is chosen\. Choose a user and the **Delete user** button and the **Open Studio** link for that user are enabled\.

1. Choose **Open Studio**\. The **Amazon SageMaker Studio** loading page displays\.

   When Amazon SageMaker Studio opens, you can start using Studio\.

Now that you've onboarded to Amazon SageMaker Studio, use the following steps to subsequently access Studio\.

**Access Studio after you onboard**

1. Open the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Amazon SageMaker Studio** at the top left of the page\.

1. On the **Amazon SageMaker Studio Control Panel**, choose your user name and then choose **Open Studio**\.

For information about using Amazon SageMaker Studio, see [Get Started with Studio](gs-studio.md)\.