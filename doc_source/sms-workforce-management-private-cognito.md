# Manage a Private Workforce \(Amazon Cognito Console\)<a name="sms-workforce-management-private-cognito"></a>

A private workforce corresponds to a single **Amazon Cognito user pool**\. Private work teams correspond to **Amazon Cognito user groups** within that user pool\. Workers correspond to **Amazon Cognito users** within those groups\. 

After your workforce has been created, you can add work teams and individual workers through the Amazon Cognito console\. You can also delete workers from your private workforce or remove them from individual teams in the Amazon Cognito console\. 

**Important**  
You can't delete work teams from the Amazon Cognito console\. Deleting a Amazon Cognito user group that is associated with an Amazon SageMaker work team will result in an error\. To remove work teams, use the SageMaker console\.  

## Create Work Teams \(Amazon Cognito Console\)<a name="create-work-teams-cog"></a>

 You can create a new work team to complete a job by adding a Amazon Cognito user group to the user pool associated with your private workforce\. To add a Amazon Cognito user group to an existing worker pool, see [Adding groups to a User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-user-groups.html)\.  

**To create a work team using an existing Amazon Cognito user group**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. In the navigation pane, choose **Workforces**\. 

1. For **Private teams**, choose **Create private team**\. 

1. Under **Team details**, give the team a name\. The name must be unique in your account in an AWS Region\. 

1. For **Add workers, **choose **Import existing Amazon Cognito user groups**, and choose one or more user groups that are part of the new team\. 

1. If you choose an **SNS topic**, all workers added to the team are subscribed to the Amazon Simple Notification Service \(Amazon SNS\) topic and notified when new work items are available to the team\. Choose from a list of your existing SNS topics related to SageMaker Ground Truth or Amazon Augmented AI or choose **Create new topic** to create one\. 
**Note**  
Amazon SNS notifications are supported by Ground Truth and are not supported by Augmented AI\. If you subscribe workers to receive SNS notifications, they only receive notifications about Ground Truth labeling jobs\. They do not receive notifications about Augmented AI tasks\. 

### Subscriptions<a name="subscriptions-cog-workteam"></a>

After you have created a work team, you can see more information about the team and change or set the SNS topic to which its members are subscribed using the Amazon Cognito console\. If you added any team members before you subscribed the team to a topic, you need to manually subscribe those members to that topic\. For more information, see [Create and manage Amazon SNS topics for your work teams](sms-workforce-management-private-sns.md)\. 

## Add and Remove Workers \(Amazon Cognito Console\)<a name="add-remove-workers-cog"></a>

 When using the Amazon Cognito console to add workers to a work team, you must add a user to the user pool associated with the workforce before adding that user to a user group\. Users can be added to a user pool in various ways\. For more information, see [Signing Up and Confirming User Accounts](https://docs.aws.amazon.com/cognito/latest/developerguide/signing-up-users-in-your-app.html)\. 

### Add a Worker to a Work Team<a name="add-worker-workteam-cog"></a>

After a user has been added to a pool, the user can be associated with user groups inside of that pool\. After a user has been added to a user group, that user becomes a worker on any work team created using that user group\.

**To add a user to a user group**

1. Open the Amazon Cognito console: [https://us\-east\-2\.console\.aws\.amazon\.com/Amazon Cognito/home](https://us-east-2.console.aws.amazon.com/cognito/home)\. 

1. Choose **Manage User Pools**\.

1. Choose the user pool associated with your SageMaker workforce\.  

1. Under **General Settings**, choose **Users and Groups** and do one of the following: 
   + Choose **Groups**, choose the group that you want to add the user to, and choose **Add users**\. Choose the users that you want to add by choosing the plus\-icon to the right of the user's name\.  
   + Choose **Users**, choose the user that you want to add to the user group, and choose **Add to group**\. From the dropdown menu, choose the group and choose **Add to group**\.

### Disable and Remove a Worker From a Work Team<a name="disable-remove-workers-cog"></a>

Disabling a worker stops the worker from receiving jobs\. This action doesn't remove the worker from the workforce, or from any work team the worker is associated with\. To remove a user from a work team in Amazon Cognito, you remove the user from the user group associated with that team\.

**To deactivate a worker  \(Amazon Cognito console\)**

1. Open the Amazon Cognito console: [https://us\-east\-2\.console\.aws\.amazon\.com/Amazon Cognito/home](https://us-east-2.console.aws.amazon.com/cognito/home)\. 

1. Choose **Manage User Pools**\.

1. Choose the user pool associated with your SageMaker workforce\.

1. Under **General Settings**, choose **Users and Groups**\.

1. Choose the user that you want to disable\.

1. Choose **Disable User**\.

You can enable a disabled user by choosing **Enable User**\.  

**To remove a user from a user group \(Amazon Cognito console\)**

1. Open the Amazon Cognito console: [https://us\-east\-2\.console\.aws\.amazon\.com/cognito/home](https://us-east-2.console.aws.amazon.com/cognito/home)\. 

1. Choose **Manage User Pools**\. 

1. Choose the user pool associated with your SageMaker workforce\.  

1. Under **General Settings**, choose **Users and Groups**\. 

1. For **User** tab, choose the **X** icon to the right of the group from which you want to remove the user\. 