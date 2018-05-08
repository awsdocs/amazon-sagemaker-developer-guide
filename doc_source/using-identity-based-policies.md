# Using Identity\-based Policies \(IAM Policies\) for Amazon SageMaker<a name="using-identity-based-policies"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on Amazon SageMaker resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your Amazon SageMaker resources\. For more information, see [Overview of Managing Access Permissions to Your Amazon SageMaker Resources](access-control-overview.md)\. 

**Topics**
+ [Permissions Required to Use the Amazon SageMaker Console](#console-permissions)
+ [AWS Managed \(Predefined\) Policies for Amazon SageMaker](#access-policy-aws-managed-policies)

The following is an example of a basic permissions policy:

```
{
   "Version": "2012-10-17",
   "Statement": [{
      "Sid": "AllowCreate-Describe-Delete-Models",
      "Effect": "Allow",
      "Action": [
         "sagemaker:CreateModel",
         "sagemaker:DescribeModel",
         "sagemaker:DeleteModel"],
      "Resource": "*"
      }
   ],
     "Statement": [{
      "Sid": "AdditionalIamPermission",
      "Effect": "Allow",
      "Action": [
         "iam:PassRole"],
      "Resource": "arn:aws:iam::account-id:role/role-name"
      }
   ]
}
```

The policy has two statements:
+ The first statement grants permission for three Amazon SageMaker actions \(`sagemaker:CreateModel`, `sagemaker:DescribeModel`, and `sagemaker:DeleteModel`\) within an Amazon SageMaker notebook instance\. Using the wildcard character \(\*\) as the resource grants universal permissions for these actions across all AWS Regions and models owned by this account\.
+ The second statement grants permission for the `iam:PassRole` action, which is needed for the Amazon SageMaker action `sagemaker:CreateModel`, which is allowed by the first statement\.

The policy doesn't specify the `Principal` element because in an identity\-based policy you don't specify the principal who gets the permission\. When you attach the policy to a user, the user is the implicit principal\. When you attach a permissions policy to an IAM role, the principal identified in the role's trust policy gets the permissions\. 

For a table showing all of the Amazon SageMaker API operations and the resources that they apply to, see [Amazon SageMaker API Permissions: Actions, Permissions, and Resources Reference](api-permissions-reference.md)\. 

## Permissions Required to Use the Amazon SageMaker Console<a name="console-permissions"></a>

The permissions reference table lists the Amazon SageMaker API operations and shows the required permissions for each operation\. For more information about Amazon SageMaker API operations, see [Amazon SageMaker API Permissions: Actions, Permissions, and Resources Reference](api-permissions-reference.md)\.

To use the Amazon SageMaker console, you need to grant permissions for additional actions\. Specifically, the console needs permissions that allow the `ec2` actions to display subnets, VPCs, and security groups\. Optionally, the console needs permission to create *execution roles* for tasks such as `CreateNotebook`, `CreateTrainingJob`, and `CreateModel`\. Grant these permissions with the following permissions policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        // Populate customer VPCs, Subnets, and Security Groups for CreateNotebookInstance form
        // These permissions needed to create the notebook instance in the console
        {
          "Sid": "CreateNotebookInstanceForm",
          "Effect": "Allow",
          "Action": [
            "ec2:DescribeVpcs",
            "ec2:DescribeSubnets",
            "ec2:DescribeSecurityGroups"
          ],
          "Resource": "*"
        },
        // Create execution roles for CreateNotebookInstance, CreateTrainingJob, and CreateModel
        // Needed if creating an IAM role (for example, as part of creating a notebook instance)
        {
          "Sid": "CreateExecutionRoles",
          "Effect": "Allow",
          "Action": [
            "iam:CreateRole",
            "iam:CreatePolicy",
            "iam:AttachRolePolicy"
          ],
          "Resource": "*"
        }
    ]
}
```

## AWS Managed \(Predefined\) Policies for Amazon SageMaker<a name="access-policy-aws-managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate which permissions are needed\. For more information, see [AWS Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to Amazon SageMaker:
+ **AmazonSageMakerReadOnly** – Grants read\-only access to Amazon SageMaker resources\. 
+ **AmazonSageMakerFullAccess** – Grants full access to Amazon SageMaker resources and the supported operations\. \(This does not provide unrestricted S3 access, but supports buckets/objects with specific sagemaker tags\.\)

The following AWS managed policies can also be attached to users in your account:
+ [AdministratorAccess](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_administrator) – Grants all actions for all AWS services and for all resources in the account\. 
+ [DataScientist](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_data-scientist) – Grants a wide range of permissions to cover most of the use cases \(primarily for analytics and business intelligence\) encountered by data scientists\.

You can review these permissions policies by signing in to the IAM console and searching for them\.

You can also create your own custom IAM policies to allow permissions for Amazon SageMaker actions and resources as you need them\. You can attach these custom policies to the IAM users or groups that require them\. 