# Use a Private Workforce<a name="sms-workforce-private"></a>

 A **private workforce** is a group of workers that *you* choose\. These can be employees of your company or a group of subject matter experts from your industry\. For example, if the task is to label medical images, you could create a private workforce of people knowledgeable about the images in question\. 

Each AWS account has access to a single private workforce per region, and the owner has the ability to create multiple **private** **work teams** within that workforce\. A single private work team is used to complete a labeling job or human review task, or a *job*\. You can assign each work team to a separate job or use a single team for multiple jobs\. A single worker can be in more than one work team\. 

Your private workforce can either be created and managed using [Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) or your own private OpenID Connect \(OIDC\) Identity Provider \(IdP\)\. 

If you are a new user of [Amazon SageMaker Ground Truth](https://docs.aws.amazon.com/sagemaker/latest/dg/sms.html) or [Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-use-augmented-ai-a2i-human-review-loops.html) and do not require your workers to be managed with your own IdP, it is recommended that you use Amazon Cognito to create and manage your private workforce\. 

After you create a workforce, in addition to creating and managing work teams, you can do the following: 
+ [Track worker performance](https://docs.aws.amazon.com/sagemaker/latest/dg/workteam-private-tracking.html)
+ [Create and manage Amazon SNS topics](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management-private-sns.html) to notify workers when labeling tasks are available
+ [Manage Private Workforce Access to Tasks Using IP Addresses](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management-private-api.html)

**Note**  
Your private workforce is shared between Ground Truth and Amazon A2I\. To create and manage private work teams used by Augmented AI, use the Ground Truth section of the SageMaker console\. 

**Topics**
+ [Create and Manage Amazon Cognito Workforce](sms-workforce-private-use-cognito.md)
+ [Create and Manage OIDC IdP Workforce](sms-workforce-private-use-oidc.md)
+ [Manage Private Workforce Using the Amazon SageMaker API](sms-workforce-management-private-api.md)
+ [Track Worker Performance](workteam-private-tracking.md)
+ [Create and manage Amazon SNS topics for your work teams](sms-workforce-management-private-sns.md)