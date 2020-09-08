# Manage a Workforce \(Amazon SageMaker Console\)<a name="sms-workforce-management-private-console"></a>

You can use the Amazon SageMaker console to create and manage the work teams and individual workers that make up a private workforce\. 

Use a work team to assign members of your private workforce to a labeling or human review *job*\. When you create your workforce using the SageMaker console, there is a work team called **Everyone\-in\-private\-workforce** that enables you to assign your entire workforce to a job\. Because an imported Amazon Cognito user pool may contain members that you don't want to include in your work teams, a similar work team is not created for Amazon Cognito user pools\.

 You have two choices to create a new work team: 
+ You can create a work team in the SageMaker console and add members from your workforce to the team\. 
+ You can create a user group by using the Amazon Cognito console and then create a work team by importing the user group\. You can import more than one user group into each work team\. You manage the members of the work team by updating the user group in the Amazon Cognito console\. See [Manage a Private Workforce \(Amazon Cognito Console\)](sms-workforce-management-private-cognito.md) for more information\.  

## Create a Work Team Using the SageMaker Console<a name="create-workteam-sm-console"></a>

You can create a new Amazon Cognito user group or import an existing user group using the SageMaker console, on the **Labeling workforces** page\. For more information on creating a user group in the Amazon Cognito console, see [Manage a Private Workforce \(Amazon Cognito Console\)](sms-workforce-management-private-cognito.md)\.

**To create a work team using the SageMaker console**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) 

1. Choose **Labeling workforces** from the left menu\.

1.  Under **Private**, choose **Create private team**\. 

1. Under **Team details**, enter a **Team name**\. The name must be unique in your account in an AWS Region\. 

1. Under **Add workers**, choose a method to add workers to the team using a user group\.
   + If you chose **Create a team by adding workers to a new Amazon Cognito user group**, select the workers to add to the team\. 
   + If you chose **Create a team by importing existing Amazon Cognito user groups**, choose the user groups that are part of the new team\. 

1. If you select an **SNS topic**, all workers added to the team are subscribed to the Amazon SNS topic and notified when new work items are available to the team\. Select from a list of your existing Ground Truth related Amazon SNS topics or select **Create new topic** to open a topic\-creation dialog\. 

   Amazon SNS notifications are supported by Ground Truth and are not supported by Augmented AI\. If you subscribe workers to receive SNS notifications, they only receive notifications about Ground Truth labeling jobs\. They do not receive notifications about Augmented AI tasks\. 

Workers in a workteam subscribed to a topic receive notifications when a new Ground Truth labeling job for that team becomes available and when one is about to expire\. 

 Read [Create and manage Amazon SNS topics for your work teams](sms-workforce-management-private-sns.md) for more information about using Amazon SNS topic\.

### Subscriptions<a name="subscriptions"></a>

After you have created a work team, you can see more information about the team and change or set the Amazon SNS topic to which its members are subscribed by visiting the Amazon Cognito console\. If you added any team members before you subscribed the team to a topic, you need to manually subscribe those members to that topic\. Read [Create and manage Amazon SNS topics for your work teams](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management-private-sns.html) for more information on creating and managing the Amazon SNS topic\. 

## Add or Remove Workers<a name="add-remove-workers-sm"></a>

 A *work team* is a group of workers within your workforce to whom you can assign jobs\. A worker can be added to more than one work team\. Once a worker has been added to a work team, that worker can be disabled or removed\.

### Add Workers to the Workforce<a name="add-workers-sm-console"></a>

 Adding a worker to the workforce enables you to add that worker to any work team within that work force\.  

**To add workers using the private workforce summary page**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose** Labeling workforces** to navigate to your private workforce summary page\. 

1. Choose **Private**\.

1. Choose **Invite new workers**\.

1. Paste or type a list of email addresses, separated by commas, into the email addresses box\. You can have up to 50 email addresses in this list\. 

### Add a Worker to a Work Team<a name="add-worker-workteam-sm-console"></a>

 A worker must be added to the workforce before being added to a work team\. To add a worker to a work team, first navigate to the **Private workforce summary** page using the steps above\. 

**To add a worker to a work team from the private workforce summary page**

1. In the **Private teams** section, choose the team to which you want to add the workers\. 

1. Choose the **Workers** tab\. 

1. Choose **Add workers to team** and choose the boxes next to the workers that you want to add\.

1. Click **Add workers to team**\.

### Disable and Remove a Worker from the Workforce<a name="disable-remove-workers-console"></a>

Disabling a worker stops the worker from receiving a job\. This action does not remove the worker from the workforce, or from any work team with which the worker is associated\. To disable or remove a worker from a work team, first navigate to the private workforce summary page using the steps above\. 

**To deactivate a worker using the private workforce summary page**

1. In the **Workers** section, choose the worker that you would like to disable\. 

1. Choose **Disable**\. 

 If desired, you can subsequently **Enable** a worker after they have been disabled\. 

You can remove workers from your private workforce directly in the SageMaker console if that worker was added in this console\. If you added the worker \(user\) in the Amazon Cognito console, see [Manage a Private Workforce \(Amazon Cognito Console\)](sms-workforce-management-private-cognito.md) to learn how to remove the worker in the Amazon Cognito console\. 

**To remove a worker using the private workforce summary page**

1. In the **Workers** section, choose the worker that you would like to delete\. 

1. If the worker has not been disabled, choose **Disable**\.  

1. Select the worker and choose **Delete**\. 