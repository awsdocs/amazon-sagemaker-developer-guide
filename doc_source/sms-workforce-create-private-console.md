# Create a Private Workforce \(Amazon SageMaker Console\)<a name="sms-workforce-create-private-console"></a>

 You can create a private workforce in the Amazon SageMaker console in one of two ways:
+ When creating a labeling job in the **Labeling jobs** page of the Amazon SageMaker Ground Truth section\.
+ Using the **Labeling workforces** page of the Amazon SageMaker Ground Truth section\. If you are creating a private workforce for an Amazon A2I human review workflow, use this method\.

Both of these methods also create a default work team containing all of the members of the workforce\. This private workforce is available to use for both Ground Truth and Amazon Augmented AI jobs\. 

When you create a private workforce using the console, SageMaker uses Amazon Cognito as an identity provider for your workforce\. If you want to use your own OpenID Connect \(OIDC\) Identity Provider \(IdP\) to create and manage your private workforce, you must create a workforce using the SageMaker API operation `CreateWorkforce`\. To learn more, see [Create a Private Workforce \(OIDC IdP\)](sms-workforce-create-private-oidc.md)\. 

## Create an Amazon Cognito Workforce When Creating a Labeling Job<a name="create-workforce-labeling-job"></a>

If you haven't created a private workforce when you create your labeling job and you choose to use private workers, you are prompted to create a work team\. This will create a private workforce using Amazon Cognito\.

**To create a workforce while creating a labeling job \(console\)**

1.  Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, choose **Labeling jobs** and fill in all required fields\. For instructions on how to start a labeling job, see [Getting started](sms-getting-started.md)\. Choose **Next**\.

1. Choose **Private** for the workforce type\. 

1. In the **Workers** section, enter:

   1. The **Team name**\. 

   1. Email addresses for up to 100 workforce members\. Email addresses are case sensitive\. Your workers must log in using the same case used when the address was initially entered\. You can add additional workforce members after the job has been created\. 

   1. The name of your organization\. SageMaker uses this to customize the email sent to the workers\.

   1. A contact email address for workers to report issues related to the task\.

When you create the labeling job, an email is sent to each worker inviting them to join the workforce\. After creating the workforce, you can add, delete, and disable workers using the SageMaker console or the Amazon Cognito console\. 

## Create an Amazon Cognito Workforce Using the Labeling Workforces Page<a name="create-workforce-sm-console"></a>

To create and manage your private workforce using Amazon Cognito, you can use the **Labeling workforces** page\. When following the instructions below, you have the option to create a private workforce by entering worker emails importing a pre\-existing workforce from an Amazon Cognito user pool\. To import a workforce, see [Create a Private Workforce \(Amazon Cognito Console\)](sms-workforce-create-private-cognito.md)\.

**To create a private workforce using worker emails**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. In the navigation pane, choose **Labeling workforces**\. 

1. Choose **Private**, then choose **Create private team**\. 

1. Choose **Invite new workers by email**\.

1. Paste or type a list of up to 50 email addresses, separated by commas, into the email addresses box\. 

1. Enter an organization name and contact email\. 

1. Optionally, choose an SNS topic to which to subscribe the team so workers are notified by email when new Ground Truth labeling jobs become available\. Amazon SNS notifications are supported by Ground Truth and are not supported by Augmented AI\. If you subscribe workers to receive SNS notifications, they only receive notifications about Ground Truth labeling jobs\. They do not receive notifications about Augmented AI tasks\. 

1.  Click the **Create private team** button\. 

After you import your private workforce, refresh the page\. On the **Private workforce summary** page, you can see information about the Amazon Cognito user pool for your workforce, a list of work teams for your workforce, and a list of all of the members of your private workforce\. 

**Note**  
If you delete all of your private work teams, you have to repeat this process to use a private workforce in that region\. 