# Managing Your Workforce<a name="sms-workforce-management"></a>

A *workforce* is the group of workers that you have selected to label your dataset\. You can choose either the Amazon Mechanical Turk workforce, a vendor\-managed workforce, or you can create your own private workforce to label your dataset\. Whichever you choose, Amazon SageMaker Ground Truth takes care of sending tasks to workers\. 

When you use a private workforce, you also create *work teams*, a group of workers from your workforce that are assigned to specific labeling jobs\. You can have multiple work teams and can assign one or more work teams to each labeling job\.

Ground Truth uses Amazon Cognito to manage your workforce and work teams\. For more information about the permissions required to manage this way, see [Permissions Required to Use the Amazon SageMaker Ground Truth Console](using-identity-based-policies.md#groundtruth-console-policy)\.

**Topics**
+ [Using the Amazon Mechanical Turk Workforce](sms-workforce-management-public.md)
+ [Managing Vendor Workforces](sms-workforce-management-vendor.md)
+ [Managing a Private Workforce](sms-workforce-management-private.md)
+ [Create and manage Amazon SNS topics for your work teams](sms-workforce-management-private-sns.md)