# Manage a Private Workforce<a name="sms-workforce-management-private"></a>

After you have created a private workforce, you can do the following using either the Amazon SageMaker or Amazon Cognito console\. Note, if you add workers to a workforce using the Amazon Cognito console, you must use the same console to remove the worker from the workforce\.
+ Add work teams to your workforce
+ Add workers to your workforce and one or more work teams
+ Disable or remove workers from your workforce and one or more workteams 

You can restrict access to tasks to workers at specific IP addresses using the Amazon SageMaker API\. For more information, see [Manage Private Workforce Access to Tasks Using IP Addresses](sms-workforce-management-private-api.md)\.

**Note**  
Your private workforce is shared between Amazon SageMaker Ground Truth and Amazon Augmented AI\. To manage private work teams and workers used by Amazon Augmented AI, use the Ground Truth section of the Amazon SageMaker console or the Amazon Cognito user pool that you used to create your shared private workforce\. 

**Topics**
+ [Manage a Private Workforce \(Amazon SageMaker Console\)](sms-workforce-management-private-console.md)
+ [Manage a Private Workforce \(Amazon Cognito Console\)](sms-workforce-management-private-cognito.md)
+ [Manage Private Workforce Access to Tasks Using IP Addresses](sms-workforce-management-private-api.md)
+ [Track Worker Performance](workteam-private-tracking.md)