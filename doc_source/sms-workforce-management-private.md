# Managing a Private Workforce<a name="sms-workforce-management-private"></a>

A *private workforce* is a group of workers that you choose\. These can be employees of your company or a group of subject matter experts from your industry\. For example, if the task is to label medical images, you could create a private workforce of people knowledgeable about the images in question\.

You create *work teams* from your private workforce\. If desired, you can assign each work team to a separate labeling job\. A single worker can be in more than one work team\.

Amazon SageMaker Ground Truth uses Amazon Cognito to define your private workforce and your work teams\. Amazon Cognito is a service that you can use to create identities for your workers and authenticate these identities with identity providers\. You can use providers such as the following: 
+ Amazon Cognito identity provider
+ Social sign\-in providers such as Facebook and Google
+ OpenID Connect \(OIDC\) providers
+ Security Assertion Markup Language \(SAML\) providers such as Active Directory

After your workers are set up, you use Amazon Cognito to manage them\. For more information about Amazon Cognito, see [What Is Amazon Cognito?](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)

Your workers are organized into two groups\. The *workforce* is the entire set of workers that are available to work on your labeling jobs\. The workforce corresponds to an Amazon Cognito user pool\. A *work team* is a group of workers within your workforce that you can assign jobs to\. The work team corresponds to an Amazon Cognito user group\. A worker can be in more than one work team\.

**Note**  
You can only use one Amazon Cognito user pool as your Ground Truth labeling workforce\. If you plan on using an existing Amazon Cognito user pool for your Ground Truth workforce, be sure to import the user pool and create a work team before you create your first labeling job\. 

For more information, see [Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)\.

## Creating a private workforce<a name="workforce-private-create"></a>

There are three ways that you can create a private workforce: 

1. Import an existing Amazon Cognito user pool before you create your first labeling job\.

1. Use the Amazon SageMaker console to create a new Amazon Cognito user pool before you create your first labeling job\.

1. Create a new Amazon Cognito user pool while you are creating your first labeling job\.

If you are using a pre\-existing Amazon Cognito user pool for your private workforce, you must import the user pool into Ground Truth and create at least one work team, either by adding work team members in the Amazon SageMaker console or by importing an Amazon Cognito work group before you create your first labeling job for your private workforce\. 

After you create or import your private workforce you will see the **Private workforce summary** screen\. On this screen you can see information about the Amazon Cognito user pool for your workforce, a list of work teams for your workforce, and a list of all of the members of your private workforce\.

If you created your workforce using the Amazon SageMaker console, you can manage the members of your workforce in the Amazon SageMaker console or in the Amazon Cognito console\. For example, you can use the Amazon SageMaker console to add, delete, and disable workers in the pool\. If you imported the workforce from an existing Amazon Cognito user pool, you must use the Amazon Cognito console to manage the workforce\.

### Creating a workforce when creating a labeling job<a name="create-inline"></a>

If you have not created a private workforce when you create your labeling job, you are prompted to create one\. Creating the workforce also creates a default work team containing all of the members of the workforce\. If you have already created a private workforce, you instead enter the work team that should handle the labeling job\.

You provide the following information to create the workforce and work team\.
+ Up to 100 email addresses of the workforce members\. Email addresses are case\-sensitive\. Your workers must log in using the same case as the address was initially entered\. You can add additional workforce members after the job is created\.
+ The name of your organization\. This is used to customize the email sent to the workers\.
+ A contact email address for workers to report issues related to the task\.

When you create the labeling job an email is sent to each worker inviting them to join the workforce\. Once the workforce is created, you can add, delete, and disable workers using the Amazon SageMaker console or the Amazon Cognito console\.

### Creating a workforce using the console<a name="create-console"></a>

You can add up to 100 workers when you create the workforce, and then add additional workers if necessary\.

**To create a private workforce using the console**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Labeling workforces** from the left menu\.

1. Under **Add workers using Amazon Cognito**, choose **Create a new Amazon Cognito user pool with worker email addresses**\.

1. Under **Email addresses**, add the email addresses of your private workforce\. You can add up to 100 addresses\. Email addresses are case\-sensitive\. Your workers must log in using the same case as the address was initially entered\. If you want to add more addresses, you can update the workforce to include them later\.

1. Under **Invitation content**, enter information to customize the email that is sent to your private workforce to invite them to participate\. Choose **Preview invitation** to see a preview of the email that is sent to invite your workforce\.

1. Choose **Create private workforce** to start creating your private workforce\.

After you have created the workforce, Ground Truth automatically creates a work team called **Everyone\-in\-private\-workforce**\. You can use this work team to assign a labeling job to your entire workforce\.

## Creating a Work Team<a name="workteam-private-create"></a>

Use a work team to assign members of your private workforce to a labeling job\. When you create your workforce using the Amazon SageMaker Ground Truth console, there is a work team called **Everyone\-in\-private\-workforce** that you can use if you want to assign your entire workforce to a labeling job\. Because an imported Amazon Cognito user pool may contain members that you don't want to include in your work teams, a similar work team is not created for Amazon Cognito user pools\. 

You have two choices to create a new work team\. You can create a user group by using the Amazon Cognito console and then create a work team by importing the user group\. You can import more than one user group into each work team\. You manage the members of the work team by updating the user group in the Amazon Cognito console\. 

You can also create a work team in the Ground Truth console and add members from your workforce to the team\. 

**To create a work team using the Ground Truth console**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Labeling workforces** from the left menu\.

1. Under **Private teams**, choose **Create private team**\.

1. Under **Team details**, give the team a name\. The name must be unique in your account in an AWS Region\.

1. Under **Create a team using Amazon Cognito user groups**, choose the method to use to create the group\.

1. If you chose **Create a team by adding workers to a new Amazon Cognito user group**, select the workers to add to the team\.

   If you chose **Create a team by importing existing Amazon Cognito user groups**, choose the user groups that are part of the new team\.

1. If you select an **SNS topic**, all workers added to the team are subscribed to the Amazon SNS topic and notified when new work items are available to the team\. Select from a list of your existing Ground Truth related Amazon SNS topics or select **Create new topic** to open a topic\-creation dialog\.

   Workers in a workteam subscribed to a topic receive notifications when a new job for that team becomes available and when one is about to expire\.

After you have created a work team, you can choose its name in the list of work teams to see more information about the team and change or set the Amazon SNS topic to which its members are subscribed\. Teams made from more than one imported user group must be edited in the Amazon Cognito console; otherwise you can add and remove workers using the team detail page\.