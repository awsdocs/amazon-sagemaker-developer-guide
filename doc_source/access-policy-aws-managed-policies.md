# AWS Managed \(Predefined\) Policies for Amazon SageMaker<a name="access-policy-aws-managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate which permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to Amazon SageMaker:
+ **AmazonSageMakerReadOnly** – Grants read\-only access to Amazon SageMaker resources\. 
+ **AmazonSageMakerFullAccess** – Grants full access to Amazon SageMaker resources and the supported operations\. \(This does not provide unrestricted S3 access, but supports buckets/objects with specific sagemaker tags\.\)

The following AWS managed policies can also be attached to users in your account:
+ [AdministratorAccess](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_administrator) – Grants all actions for all AWS services and for all resources in the account\. 
+ [DataScientist](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_data-scientist) – Grants a wide range of permissions to cover most of the use cases \(primarily for analytics and business intelligence\) encountered by data scientists\.

You can review these permissions policies by signing in to the IAM console and searching for them\.

You can also create your own custom IAM policies to allow permissions for Amazon SageMaker actions and resources as you need them\. You can attach these custom policies to the IAM users or groups that require them\. 