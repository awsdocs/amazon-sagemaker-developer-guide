# Using Identity\-based Policies \(IAM Policies\) for Amazon SageMaker<a name="using-identity-based-policies"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on Amazon SageMaker resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your Amazon SageMaker resources\. For more information, see [Overview of Managing Access Permissions to Your Amazon SageMaker Resources](access-control-overview.md)\. 

**Topics**
+ [Permissions Required to Use the Amazon SageMaker Console](#console-permissions)
+ [Permissions Required to Use the Amazon SageMaker Ground Truth Console](#groundtruth-console-policy)
+ [AWS Managed \(Predefined\) Policies for Amazon SageMaker](#access-policy-aws-managed-policies)
+ [Control Access to Amazon SageMaker Resources by Using Tags](#access-tag-policy)
+ [Control Access to the Amazon SageMaker API by Using Identity\-based Policies](#api-access-policy)

The following is an example of a basic permissions policy:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
      "Sid": "AllowCreate-Describe-Delete-Models",
      "Effect": "Allow",
      "Action": [
         "sagemaker:CreateModel",
         "sagemaker:DescribeModel",
         "sagemaker:DeleteModel"],
      "Resource": "*"
      },
      {
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
        // SageMaker API's
        {
          "Sid": "SageMakerApis",
          "Effect": "Allow",
          "Action": [
            "sagemaker:*"
          ],
          "Resource": "*"
        },
        // Populate customer VPCs, Subnets, and Security Groups for Create forms
        {
          "Sid": "VpcConfigurationForCreateForms",
          "Effect": "Allow",
          "Action": [
            "ec2:DescribeVpcs",
            "ec2:DescribeSubnets",
            "ec2:DescribeSecurityGroups"
          ],
          "Resource": "*"
        },
        // Populate customer KMS keys for Create forms
        {
            "Sid":"KmsKeysForCreateForms",
            "Effect":"Allow",
            "Action":[
              "kms:DescribeKey",
              "kms:ListAliases"
            ],
            "Resource":"*"
        },
        // View Subscriptions in AWS Marketplace for Algorithms and ModelPackages.
        {
          "Sid": "AccessAwsMarketplaceSubscritions",
          "Effect": "Allow",
          "Action": [
            "aws-marketplace:ViewSubscriptions"
          ],
          "Resource": "*"
        },
        // View and create CodeCommit Repositories
        {
          "Effect": "Allow",
          "Action": [
            "codecommit:BatchGetRepositories",
            "codecommit:CreateRepository",
            "codecommit:GetRepository",
            "codecommit:ListRepositories",
            "codecommit:ListBranches",
            "secretsmanager:CreateSecret",
            "secretsmanager:DescribeSecret",
            "secretsmanager:ListSecrets"
          ],
          "Resource": "*"
        },
        // List/create execution roles for Create forms
        {
          "Sid":"ListAndCreateExecutionRoles",
          "Effect":"Allow",
          "Action":[
            "iam:ListRoles",
            "iam:CreateRole",
            "iam:CreateRole",
            "iam:CreatePolicy",
            "iam:AttachRolePolicy"
          ],
          "Resource":"*"
        },
        // Permissions required for ECR
        {
          "Sid": "DescribeECRMetaData",
          "Effect": "Allow",
          "Action": [
              "ecr:Describe*"
          ],
          "Resource": "*"
        },
        // PassRole permissions as required by CreateNotebookInstance, CreateTrainingJob, and CreateModel.
        {
          "Sid": "PassRoleForExecutionRoles",
          "Effect": "Allow",
          "Action": [
            "iam:PassRole"
          ],
          "Resource": "*",
          "Condition": {
            "StringEquals": {
                "iam:PassedToService": "sagemaker.amazonaws.com"
            }
          }
        }
    ]
}
```

## Permissions Required to Use the Amazon SageMaker Ground Truth Console<a name="groundtruth-console-policy"></a>

To use the Amazon SageMaker Ground Truth console, you need to grant permissions for additional resources\. Specifically, the console needs permissions for the AWS Marketplace to view subscriptions, Amazon Cognito operations to manage your private workforce, Amazon S3 actions for access to your input and output files, and AWS Lambda actions to list and invoke functions\. Grant these permissions with the following permissions policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GroundTruthConsole",
            "Effect": "Allow",
            "Action": [
                "aws-marketplace:DescribeListings",
                "aws-marketplace:ViewSubscriptions",
                
                "cognito-idp:AdminAddUserToGroup",
                "cognito-idp:AdminCreateUser",
                "cognito-idp:AdminDeleteUser",
                "cognito-idp:AdminDisableUser",
                "cognito-idp:AdminEnableUser",
                "cognito-idp:AdminRemoveUserFromGroup",
                "cognito-idp:CreateGroup",
                "cognito-idp:CreateUserPool",
                "cognito-idp:CreateUserPoolClient",
                "cognito-idp:CreateUserPoolDomain",
                "cognito-idp:DescribeUserPool",
                "cognito-idp:DescribeUserPoolClient",
                "cognito-idp:ListGroups",
                "cognito-idp:ListIdentityProviders",
                "cognito-idp:ListUsers",
                "cognito-idp:ListUsersInGroup",
                "cognito-idp:ListUserPoolClients",
                "cognito-idp:ListUserPools",
                "cognito-idp:UpdateUserPool",
                "cognito-idp:UpdateUserPoolClient",
                
                "groundtruthlabeling:DescribeConsoleJob",
                "groundtruthlabeling:ListDatasetObjects",
                "groundtruthlabeling:RunFilterOrSampleManifestJob",
                "groundtruthlabeling:RunGenerateManifestByCrawlingJob",
                
                "lambda:InvokeFunction",
                "lambda:ListFunctions",
                
                "s3:GetObject",
                "s3:PutObject",
                "s3:SelectObjectContent"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWS Managed \(Predefined\) Policies for Amazon SageMaker<a name="access-policy-aws-managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate which permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to Amazon SageMaker:
+ **AmazonSageMakerReadOnly** – Grants read\-only access to Amazon SageMaker resources\. 
+ **AmazonSageMakerFullAccess** – Grants full access to Amazon SageMaker resources and the supported operations\. \(This does not provide unrestricted S3 access, but supports buckets/objects with specific sagemaker tags\.\)

The following AWS managed policies can also be attached to users in your account:
+ [AdministratorAccess](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_administrator) – Grants all actions for all AWS services and for all resources in the account\. 
+ [DataScientist](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_data-scientist) – Grants a wide range of permissions to cover most of the use cases \(primarily for analytics and business intelligence\) encountered by data scientists\.

You can review these permissions policies by signing in to the IAM console and searching for them\.

You can also create your own custom IAM policies to allow permissions for Amazon SageMaker actions and resources as you need them\. You can attach these custom policies to the IAM users or groups that require them\. 

## Control Access to Amazon SageMaker Resources by Using Tags<a name="access-tag-policy"></a>

Control access to groups of Amazon SageMaker resources by attaching tags to the resources and specifying `ResourceTag` conditions in IAM policies\.

**Note**  
Tag\-based policies do not work to restrict the following API calls:  
ListAlgorithms
ListCodeRepositories
ListCompilationJobs
ListEndpointConfigs
ListEndpoints
ListHyperparameterTuningJobs
ListLabelingJobs
ListLabelingJobsForWorkteam
ListModelPackages
ListModels
ListNotebookInstanceLifecycleConfigs
ListNotebookInstances
ListSubscribedWorkteams
ListTags
ListTrainingJobs
ListTrainingJobsForHyperParameterTuningJob
ListTransformJobs
ListWorkteams
Search

For example, suppose you've defined two different IAM groups, named `DevTeam1` and `DevTeam2`, in your AWS account\. Suppose also that you've created 10 notebook instances, 5 of which are used for one project, and 5 of which are used for a second project\. You want to allow members of `DevTeam1` to make API calls on notebook instances used for the first project, and members of `DevTeam2` to make API calls on notebook instances used for the second project\.

You can control access to API calls by completing the following steps:

1. Add a tag with the key `Project` and value `A` to the notebook instances used for the first project\. For information about adding tags to Amazon SageMaker resources, see [AddTags](API_AddTags.md)\. 

1. Add a tag with the key `Project` and value `B` to the notebook instances used for the second project\.

1. Create an IAM policy with a `ResourceTag` condition that denies access to the notebook instances used for the second project, and attach that policy to `DevTeam1`\. The following is an example of a policy that denies all API calls on any notebook instance that has a tag with a key of `Project` and a value of `B`:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "sagemaker:*",
         "Resource": "*"
       },
       {
         "Effect": "Deny",
         "Action": "sagemaker:*",
         "Resource": "*",
         "Condition": {
           "StringEquals": {
             "sagemaker:ResourceTag/Project": "B"
           }
         }
       },
       {
         "Effect": "Deny",
         "Action": [
           "sagemaker:CreateTags",
           "sagemaker:DeleteTags"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

   For information about creating IAM policies and attaching them to identities, see [Controlling Access Using Policies](https://docs.aws.amazon.com//IAM/latest/UserGuide/access_controlling.html) in the *AWS Identity and Access Management User Guide*\.

1. Create an IAM policy with a `ResourceTag` condition that denies access to the notebook instances used for the first project, and attach that policy to `DevTeam2`\. The following is an example of a policy that denies all API calls on any notebook instance that has a tag with a key of `Project` and a value of `A`:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "*",
         "Resource": "*"
       },
       {
         "Effect": "Deny",
         "Action": "sagemaker:*",
         "Resource": "*",
         "Condition": {
           "StringEquals": {
             "sagemaker:ResourceTag/Project": "A"
           }
         }
       },
       {
         "Effect": "Deny",
         "Action": [
           "sagemaker:CreateTags",
           "sagemaker:DeleteTags"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

### Require the Presence or Absence of Tags for API Calls<a name="resource-tag"></a>

Require the presence or absence of specific tags or specific tag values by using `RequestTag` condition keys in an IAM policy\. For example, if you want to require that every endpoint created by any member of an IAM group to be created with a tag with the key `environment` and value `dev`, create a policy as follows:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    },
    {
      "Effect": "Deny",
      "Action": "sagemaker:CreateEndpoint",
      "Resource": [
        "arn:aws:sagemaker:*:*:endpoint/*"
      ]
    {
      "Effect": "Allow",
      "Action": "sagemaker:CreateEndpoint",
      "Resource": [
        "arn:aws:sagemaker:*:*:endpoint/*"
      ],
      "Condition": {
        "StringEquals": {
          "aws:RequestTag/environment": "dev"
        }
      }
    }
  ]
}
```

### Use Tags with Hyperparameter Tuning Jobs<a name="resource-tag-tuning"></a>

You can add tags to a hyperparameter tuning job when you create the tuning job by specifying the tags as the `Tags` parameter when you call [CreateHyperParameterTuningJob](API_CreateHyperParameterTuningJob.md)\. If you do this, the tags you specify for the hyperparameter tuning job are also added to all training jobs that the hyperparameter tuning job launches\.

If you add tags to a hyperparameter tuning job by calling [AddTags](API_AddTags.md), the tags you add are also added to any training jobs that the hyperparameter tuning job launches after you call `AddTags`, but are not added to training jobs the hyperparameter tuning jobs launched before you called `AddTags`\. Similarly, when you remove tags from a hyperparameter tuning job by calling [DeleteTags](API_DeleteTags.md), those tags are not removed from training jobs that the hyperparameter tuning job launched previously\. Because of this, the tags associated with training jobs can be out of sync with the tags associated with the hyperparameter tuning job that launched them\. If you use tags to control access to a hyperparameter tuning job and the training jobs it launches, you might want to keep the tags in sync\. To make sure the tags associated with training jobs stay sync with the tags associated with the hyperparameter tuning job that launched them, first call [ListTrainingJobsForHyperParameterTuningJob](API_ListTrainingJobsForHyperParameterTuningJob.md) for the hyperparameter tuning job to get a list of the training jobs that the hyperparameter tuning job launched\. Then, call `AddTags` or `DeleteTags` for the hyperparameter tuning job and for each of the training jobs in the list of training jobs to add or delete the same set of tags for all of the jobs\. The following Python example demonstrates this:

```
tuning_job_arn = smclient.describe_hyper_parameter_tuning_job(HyperParameterTuningJobName='MyTuningJob')['HyperParameterTuningJobArn']
smclient.add_tags(ResourceArn=tuning_job_arn, Tags=[{'Key':'Env', 'Value':'Dev'}])
training_jobs = smclient.list_training_jobs_for_hyper_parameter_tuning_job(
    HyperParameterTuningJobName='MyTuningJob')['TrainingJobSummaries']
    for training_job in training_jobs:
       time.sleep(1) # Wait for 1 second between calls to avoid being throttled
       smclient.add_tags(ResourceArn=training_job['TrainingJobArn'], Tags=[{'Key':'Env', 'Value':'Dev'}])
```

## Control Access to the Amazon SageMaker API by Using Identity\-based Policies<a name="api-access-policy"></a>

To control access to Amazon SageMaker API calls and calls to Amazon SageMaker hosted endpoints, use identity\-based IAM policies\.

**Topics**
+ [Restrict Access to Amazon SageMaker API and Runtime to Calls from Within Your VPC](#api-access-policy-vpc)
+ [Limit Access to Amazon SageMaker API and Runtime Calls by IP Address](#api-ip-filter)

### Restrict Access to Amazon SageMaker API and Runtime to Calls from Within Your VPC<a name="api-access-policy-vpc"></a>

If you set up an interface endpoint in your VPC, individuals outside the VPC can still connect to the Amazon SageMaker API and runtime over the internet unless you attach an IAM policy that restricts access to calls coming from within the VPC to all users and groups that have access to your Amazon SageMaker resources\. For information about creating a VPC interface endpoint for the Amazon SageMaker API and runtime, see [Connect to Amazon SageMaker Through a VPC Interface Endpoint](interface-vpc-endpoint.md)\.

**Important**  
If you apply an IAM policy similar to one of the following, users can't access the specified Amazon SageMaker APIs through the console\.

To restrict access to only connections made from within your VPC, create an AWS Identity and Access Management policy that restricts access to only calls that come from within your VPC\. Then add that policy to every AWS Identity and Access Management user, group, or role used to access the Amazon SageMaker API or runtime\.

**Note**  
This policy allows connections only to callers within a subnet where you created an interface endpoint\.

```
{
    "Id": "api-example-1",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Enable API Access",
            "Effect": "Allow",
            "Action": [
                "sagemaker:*
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:SourceVpc": "vpc-111bbaaa"
                }
            }
        }
    ]
}
```

If you want to restrict access to the API to only calls made using the interface endpoint, use the `aws:SourceVpce` condition key instead of `aws:SourceVpc`:

```
{
    "Id": "api-example-1",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Enable API Access",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedNotebookInstanceUrl"
            ],
            "Resource": "*",
            "Condition": {
                "ForAllValues:StringEquals": {
                    "aws:sourceVpce": [
                        "vpce-111bbccc",
                        "vpce-111bbddd"
                    ]
                }
            }
        }
    ]
}
```

### Limit Access to Amazon SageMaker API and Runtime Calls by IP Address<a name="api-ip-filter"></a>

To allow access to Amazon SageMaker API calls and runtime invocations only from IP addresses in a list that you specify, attach an IAM policy that denies access to the API unless the call comes from an IP address in the list to every AWS Identity and Access Management user, group, or role used to access the API or runtime\. For information about creating IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *AWS Identity and Access Management User Guide*\. To specify the list of IP addresses that you want to have access to the API call, use the `NotIpAddress` condition operator and the `aws:SourceIP` condition context key\. For information about IAM condition operators, see [IAM JSON Policy Elements: Condition Operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html) in the *AWS Identity and Access Management User Guide*\. For information about IAM condition context keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)\.

For example, the following policy allows access to the [CreateTrainingJob](API_CreateTrainingJob.md) only from IP addresses in the ranges `192.0.2.0`\-`192.0.2.255` and `203.0.113.0`\-`203.0.113.255`:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "sagemaker:CreateTrainingJob",
            "Resource": "*",
            "Condition": {
                "NotIpAddress": {
                    "aws:SourceIp": [
                        "192.0.2.0/24",
                        "203.0.113.0/24"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "sagemaker:CreateTrainingJob",
            "Resource": "*",
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": [
                        "192.0.2.0/24",
                        "203.0.113.0/24"
                    ]
                }
            }
        }
    ]
}
```