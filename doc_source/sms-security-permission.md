# Assign IAM Permissions to Use Ground Truth<a name="sms-security-permission"></a>

Use the topics in this section to learn how to use AWS Identity and Access Management \(IAM\) managed and custom policies to manage access to Ground Truth and associated resources\. 

You can use the sections on this page to learn the following: 
+ How to create IAM policies that grant a user or role permission to create a labeling job\. Administrators can use IAM policies to restrict access to Amazon SageMaker and other AWS services that are specific to Ground Truth\.
+ How to create a SageMaker *execution role*\. An execution role is the role that you specify when you create a labeling job\. The role is used to start and manage your labeling job\.

The following is an overview of the topics you'll find on this page: 
+ If you are getting started using Ground Truth, or you do not require granular permissions for your use case, it is recommended that you use the IAM managed policies described in [Use IAM Managed Policies with Ground Truth](sms-security-permissions-get-started.md)\.
+ Learn about the permissions required to use the Ground Truth console in [Grant IAM Permission to Use the Amazon SageMaker Ground Truth Console](sms-security-permission-console-access.md)\. This section includes policy examples that grant an IAM entity permission to create and modify private work teams, subscribe to vendor work teams, and create custom labeling workflows\.
+ When you create a labeling job, you must provide an execution role\. Use [Create a SageMaker Execution Role for a Ground Truth Labeling Job](sms-security-permission-execution-role.md) to learn about the permissions required for this role\.