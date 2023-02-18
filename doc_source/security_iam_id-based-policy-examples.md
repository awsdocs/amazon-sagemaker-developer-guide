# Amazon SageMaker Identity\-Based Policy Examples<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify SageMaker resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users or groups that require those permissions\. To learn how to attach policies to an IAM user or group, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating Policies on the JSON Tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy Best Practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the SageMaker Console](#security_iam_id-based-policy-examples-console)
+ [Allow Users to View Their Own Permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Control Creation of SageMaker Resources with Condition Keys](#sagemaker-condition-examples)
+ [Control Access to the SageMaker API by Using Identity\-based Policies](#api-access-policy)
+ [Limit Access to SageMaker API and Runtime Calls by IP Address](#api-ip-filter)
+ [Limit Access to a Notebook Instance by IP Address](#nbi-ip-filter)
+ [Control Access to SageMaker Resources by Using Tags](#access-tag-policy)
+ [Require the Presence or Absence of Tags for API Calls](#resource-tag)

## Policy Best Practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies determine whether someone can create, access, or delete SageMaker resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or a root user in your AWS account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

## Using the SageMaker Console<a name="security_iam_id-based-policy-examples-console"></a>

To access the Amazon SageMaker console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the SageMaker resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(users or roles\) with that policy\.

To ensure that those entities can still use the SageMaker console, also attach the following AWS managed policy to the entities\. For more information, see [Adding Permissions to a User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*:

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that you're trying to perform\.

**Topics**
+ [Permissions Required to Use the Amazon SageMaker Console](#console-permissions)
+ [Permissions Required to Use the Amazon SageMaker Ground Truth Console](#groundtruth-console-policy)
+ [Permissions Required to Use the Amazon Augmented AI \(Preview\) Console](#amazon-augmented-ai-console-policy)

### Permissions Required to Use the Amazon SageMaker Console<a name="console-permissions"></a>

The permissions reference table lists the Amazon SageMaker API operations and shows the required permissions for each operation\. For more information about Amazon SageMaker API operations, see [Amazon SageMaker API Permissions: Actions, Permissions, and Resources Reference](api-permissions-reference.md)\.

To use the Amazon SageMaker console, you need to grant permissions for additional actions\. Specifically, the console needs permissions that allow the `ec2` actions to display subnets, VPCs, and security groups\. Optionally, the console needs permission to create *execution roles* for tasks such as `CreateNotebook`, `CreateTrainingJob`, and `CreateModel`\. Grant these permissions with the following permissions policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
          "Sid": "SageMakerApis",
          "Effect": "Allow",
          "Action": [
            "sagemaker:*"
          ],
          "Resource": "*"
        },
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
        {
            "Sid":"KmsKeysForCreateForms",
            "Effect":"Allow",
            "Action":[
              "kms:DescribeKey",
              "kms:ListAliases"
            ],
            "Resource":"*"
        },
        {
          "Sid": "AccessAwsMarketplaceSubscriptions",
          "Effect": "Allow",
          "Action": [
            "aws-marketplace:ViewSubscriptions"
          ],
          "Resource": "*"
        },
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
        {
          "Sid":"ListAndCreateExecutionRoles",
          "Effect":"Allow",
          "Action":[
            "iam:ListRoles",
            "iam:CreateRole",
            "iam:CreatePolicy",
            "iam:AttachRolePolicy"
          ],
          "Resource":"*"
        },
        {
          "Sid": "DescribeECRMetaData",
          "Effect": "Allow",
          "Action": [
              "ecr:Describe*"
          ],
          "Resource": "*"
        },
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



### Permissions Required to Use the Amazon SageMaker Ground Truth Console<a name="groundtruth-console-policy"></a>

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

### Permissions Required to Use the Amazon Augmented AI \(Preview\) Console<a name="amazon-augmented-ai-console-policy"></a>

To use the Augmented AI console, you need to grant permissions for additional resources\. Grant these permissions with the following permissions policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:*Algorithm",
                "sagemaker:*Algorithms",
                "sagemaker:*App",
                "sagemaker:*Apps",
                "sagemaker:*AutoMLJob",
                "sagemaker:*AutoMLJobs",
                "sagemaker:*CodeRepositories",
                "sagemaker:*CodeRepository",
                "sagemaker:*CompilationJob",
                "sagemaker:*CompilationJobs",
                "sagemaker:*Endpoint",
                "sagemaker:*EndpointConfig",
                "sagemaker:*EndpointConfigs",
                "sagemaker:*EndpointWeightsAndCapacities",
                "sagemaker:*Endpoints",
                "sagemaker:*Environment",
                "sagemaker:*EnvironmentVersion",
                "sagemaker:*EnvironmentVersions",
                "sagemaker:*Environments",
                "sagemaker:*Experiment",
                "sagemaker:*Experiments",
                "sagemaker:*FlowDefinitions",
                "sagemaker:*HumanLoop",
                "sagemaker:*HumanLoops",
                "sagemaker:*HumanTaskUi",
                "sagemaker:*HumanTaskUis",
                "sagemaker:*HyperParameterTuningJob",
                "sagemaker:*HyperParameterTuningJobs",
                "sagemaker:*LabelingJob",
                "sagemaker:*LabelingJobs",
                "sagemaker:*Metrics",
                "sagemaker:*Model",
                "sagemaker:*ModelPackage",
                "sagemaker:*ModelPackages",
                "sagemaker:*Models",
                "sagemaker:*MonitoringExecutions",
                "sagemaker:*MonitoringSchedule",
                "sagemaker:*MonitoringSchedules",
                "sagemaker:*NotebookInstance",
                "sagemaker:*NotebookInstanceLifecycleConfig",
                "sagemaker:*NotebookInstanceLifecycleConfigs",
                "sagemaker:*NotebookInstanceUrl",
                "sagemaker:*NotebookInstances",
                "sagemaker:*ProcessingJob",
                "sagemaker:*ProcessingJobs",
                "sagemaker:*RenderUiTemplate",
                "sagemaker:*Search",
                "sagemaker:*SearchSuggestions",
                "sagemaker:*Tags",
                "sagemaker:*TrainingJob",
                "sagemaker:*TrainingJobs",
                "sagemaker:*TransformJob",
                "sagemaker:*TransformJobs",
                "sagemaker:*Trial",
                "sagemaker:*TrialComponent",
                "sagemaker:*TrialComponents",
                "sagemaker:*Trials",
                "sagemaker:*Workteam",
                "sagemaker:*Workteams"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:*FlowDefinition"
            ],
            "Resource": "*",
            "Condition": {
                "StringEqualsIfExists": {
                    "sagemaker:WorkteamType": [
                        "private-crowd",
                        "vendor-crowd"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "application-autoscaling:DeleteScalingPolicy",
                "application-autoscaling:DeleteScheduledAction",
                "application-autoscaling:DeregisterScalableTarget",
                "application-autoscaling:DescribeScalableTargets",
                "application-autoscaling:DescribeScalingActivities",
                "application-autoscaling:DescribeScalingPolicies",
                "application-autoscaling:DescribeScheduledActions",
                "application-autoscaling:PutScalingPolicy",
                "application-autoscaling:PutScheduledAction",
                "application-autoscaling:RegisterScalableTarget",
                "aws-marketplace:ViewSubscriptions",
                "cloudwatch:DeleteAlarms",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:GetMetricData",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:ListMetrics",
                "cloudwatch:PutMetricAlarm",
                "cloudwatch:PutMetricData",
                "codecommit:BatchGetRepositories",
                "codecommit:CreateRepository",
                "codecommit:GetRepository",
                "codecommit:ListBranches",
                "codecommit:ListRepositories",
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
                "cognito-idp:ListUserPoolClients",
                "cognito-idp:ListUserPools",
                "cognito-idp:ListUsers",
                "cognito-idp:ListUsersInGroup",
                "cognito-idp:UpdateUserPool",
                "cognito-idp:UpdateUserPoolClient",
                "ec2:CreateNetworkInterface",
                "ec2:CreateNetworkInterfacePermission",
                "ec2:CreateVpcEndpoint",
                "ec2:DeleteNetworkInterface",
                "ec2:DeleteNetworkInterfacePermission",
                "ec2:DescribeDhcpOptions",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribeRouteTables",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcEndpoints",
                "ec2:DescribeVpcs",
                "ecr:BatchCheckLayerAvailability",
                "ecr:BatchGetImage",
                "ecr:CreateRepository",
                "ecr:Describe*",
                "ecr:GetAuthorizationToken",
                "ecr:GetDownloadUrlForLayer",
                "elastic-inference:Connect",
                "elasticfilesystem:DescribeFileSystems",
                "elasticfilesystem:DescribeMountTargets",
                "fsx:DescribeFileSystems",
                "glue:CreateJob",
                "glue:DeleteJob",
                "glue:GetJob",
                "glue:GetJobRun",
                "glue:GetJobRuns",
                "glue:GetJobs",
                "glue:ResetJobBookmark",
                "glue:StartJobRun",
                "glue:UpdateJob",
                "groundtruthlabeling:*",
                "iam:ListRoles",
                "kms:DescribeKey",
                "kms:ListAliases",
                "lambda:ListFunctions",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams",
                "logs:GetLogEvents",
                "logs:PutLogEvents",
                "sns:ListTopics"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogDelivery",
                "logs:DeleteLogDelivery",
                "logs:DescribeResourcePolicies",
                "logs:GetLogDelivery",
                "logs:ListLogDeliveries",
                "logs:PutResourcePolicy",
                "logs:UpdateLogDelivery"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:SetRepositoryPolicy",
                "ecr:CompleteLayerUpload",
                "ecr:BatchDeleteImage",
                "ecr:UploadLayerPart",
                "ecr:DeleteRepositoryPolicy",
                "ecr:InitiateLayerUpload",
                "ecr:DeleteRepository",
                "ecr:PutImage"
            ],
            "Resource": "arn:aws:ecr:*:*:repository/*sagemaker*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "codecommit:GitPull",
                "codecommit:GitPush"
            ],
            "Resource": [
                "arn:aws:codecommit:*:*:*sagemaker*",
                "arn:aws:codecommit:*:*:*SageMaker*",
                "arn:aws:codecommit:*:*:*Sagemaker*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:ListSecrets"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:DescribeSecret",
                "secretsmanager:GetSecretValue",
                "secretsmanager:CreateSecret"
            ],
            "Resource": [
                "arn:aws:secretsmanager:*:*:secret:AmazonSageMaker-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:DescribeSecret",
                "secretsmanager:GetSecretValue"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "secretsmanager:ResourceTag/SageMaker": "true"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "robomaker:CreateSimulationApplication",
                "robomaker:DescribeSimulationApplication",
                "robomaker:DeleteSimulationApplication"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "robomaker:CreateSimulationJob",
                "robomaker:DescribeSimulationJob",
                "robomaker:CancelSimulationJob"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:AbortMultipartUpload",
                "s3:GetBucketCors",
                "s3:PutBucketCors"
            ],
            "Resource": [
                "arn:aws:s3:::*SageMaker*",
                "arn:aws:s3:::*Sagemaker*",
                "arn:aws:s3:::*sagemaker*",
                "arn:aws:s3:::*aws-glue*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:ListAllMyBuckets"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "*",
            "Condition": {
                "StringEqualsIgnoreCase": {
                    "s3:ExistingObjectTag/SageMaker": "true"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": [
                "arn:aws:lambda:*:*:function:*SageMaker*",
                "arn:aws:lambda:*:*:function:*sagemaker*",
                "arn:aws:lambda:*:*:function:*Sagemaker*",
                "arn:aws:lambda:*:*:function:*LabelingFunction*"
            ]
        },
        {
            "Action": "iam:CreateServiceLinkedRole",
            "Effect": "Allow",
            "Resource": "arn:aws:iam::*:role/aws-service-role/sagemaker.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint",
            "Condition": {
                "StringLike": {
                    "iam:AWSServiceName": "sagemaker.application-autoscaling.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:AWSServiceName": "robomaker.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "sns:Subscribe",
                "sns:CreateTopic"
            ],
            "Resource": [
                "arn:aws:sns:*:*:*SageMaker*",
                "arn:aws:sns:*:*:*Sagemaker*",
                "arn:aws:sns:*:*:*sagemaker*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::*:role/*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "sagemaker.amazonaws.com",
                        "glue.amazonaws.com",
                        "robomaker.amazonaws.com",
                        "states.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

## Allow Users to View Their Own Permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```

## Control Creation of SageMaker Resources with Condition Keys<a name="sagemaker-condition-examples"></a>

Control fine\-grained access to allow the creation of SageMaker resources by using SageMaker\-specific condition keys\. For information about using condition keys in IAM policies, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

The condition keys, along with related API actions, and links to relevant documentation are listed in [Condition Keys for SageMaker](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonsagemaker.html#amazonsagemaker-policy-keys) in the *IAM User Guide*\.

The following examples show how to use the SageMaker condition keys to control access\.

**Topics**
+ [Control Access to SageMaker Resources by Using File System Condition Keys](#access-fs-condition-keys)
+ [Restrict Training to a Specific VPC](#sagemaker-condition-vpc)
+ [Restrict Access to Workforce Types for Ground Truth Labeling Jobs and Amazon A2I Human Review Workflows](#sagemaker-condition-keys-labeling)
+ [Enforce Encryption of Input Data](#sagemaker-condition-kms)
+ [Enforce Encryption of Notebook Instance Storage Volume](#sagemaker-condition-kms-nbi)
+ [Enforce Network Isolation for Training Jobs](#sagemaker-condition-isolation)
+ [Enforce a Specific Instance Type for Training Jobs](#sagemaker-condition-instance)
+ [Enforce a Specific EI Accelerator for Training Jobs](#sagemaker-condition-ei)
+ [Enforce Disabling Internet Access and Root Access for Creating Notebook Instances](#sagemaker-condition-nbi-lockdown)

### Control Access to SageMaker Resources by Using File System Condition Keys<a name="access-fs-condition-keys"></a>

SageMaker training provides a secure infrastructure for the training algorithm to run in, but for some cases you may want increased defense in depth\. For example, you minimize the risk of running untrusted code in your algorithm, or you have specific security mandates in your organization\. For these scenarios, you can use the service\-specific condition keys in the Condition element of an IAM policy to scope down the user to specific file systems, directories, access modes \(read\-write, read\-only\) and security groups\.

**Topics**
+ [Restrict an IAM User to Specific Directories and Access Modes](#access-fs-condition-keys-ex-dirs)
+ [Restrict a User to a Specific File System](#access-fs-condition-keys-ex-fs)

#### Restrict an IAM User to Specific Directories and Access Modes<a name="access-fs-condition-keys-ex-dirs"></a>

The policy below restricts a user to the `/sagemaker/xgboost-dm/train` and `/sagemaker/xgboost-dm/validation` directories of an EFS file system to `ro` \(read\-only\) AccessMode:

**Note**  
When a directory is allowed, all of its subdirectories are also accessible by the training algorithm\. POSIX permissions are ignored\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AccessToElasticFileSystem",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateHyperParameterTuningJob"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "sagemaker:FileSystemId": "fs-12345678",
                    "sagemaker:FileSystemAccessMode": "ro",
                    "sagemaker:FileSystemType": "EFS",
                    "sagemaker:FileSystemDirectoryPath": "/sagemaker/xgboost-dm/train"
                }
            }
        },
        {
            "Sid": "AccessToElasticFileSystemValidation",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateHyperParameterTuningJob"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "sagemaker:FileSystemId": "fs-12345678",
                    "sagemaker:FileSystemAccessMode": "ro",
                    "sagemaker:FileSystemType": "EFS",
                    "sagemaker:FileSystemDirectoryPath": "/sagemaker/xgboost-dm/validation"
                }
            }
        }
    ]
}
```

#### Restrict a User to a Specific File System<a name="access-fs-condition-keys-ex-fs"></a>

To prevent a malicious algorithm using a user space client from accessing any file system directly in your account, you can restrict networking traffic by allowing ingress from a specific security group\. In the following example, the user can only use the specified security group to access the file system:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AccessToLustreFileSystem",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateHyperParameterTuningJob"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "sagemaker:FileSystemId": "fs-12345678",
                    "sagemaker:FileSystemAccessMode": "ro",
                    "sagemaker:FileSystemType": "FSxLustre",
                    "sagemaker:FileSystemDirectoryPath": "/fsx/sagemaker/xgboost/train"
                },
                "ForAllValues:StringEquals": {
                    "sagemaker:VpcSecurityGroupIds": [
                        "sg-12345678"
                    ]
                }
            }
        }
    ]
}
```

Although the above example can restrict an algorithm to a specific file system, it does not prevent an algorithm from accessing any directory within that file system using the user space client\. To mitigate this, you can:
+ Ensure that the file system only contains data that you trust your users to access
+ Create an IAM role that restricts your users to launching training jobs with algorithms from approved ECR repositories

For more information on how to use roles with SageMaker, see [SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\. 

### Restrict Training to a Specific VPC<a name="sagemaker-condition-vpc"></a>

Restrict an AWS user to creating training jobs from within a Amazon VPC\. When a training job is created within a VPC, you can use VPC flow logs to monitor all traffic to and from the training cluster\. For information about using VPC flow logs, see [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) in the *Amazon Virtual Private Cloud User Guide*\.

The following policy enforces that a training job is created by a user calling [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) from within a VPC:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowFromVpc",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateHyperParameterTuningJob"
            ],
            "Resource": "*",
            "Condition": {
                "ForAllValues:StringEquals": {
                    "sagemaker:VpcSubnets": ["subnet-a1234"],
                    "sagemaker:VpcSecurityGroupIds": ["sg12345", "sg-67890"]
                },
                "Null": {
                    "sagemaker:VpcSubnets": "false",
                    "sagemaker:VpcSecurityGroupIds": "false"
                }
            }
        }

    ]
}
```

### Restrict Access to Workforce Types for Ground Truth Labeling Jobs and Amazon A2I Human Review Workflows<a name="sagemaker-condition-keys-labeling"></a>

Amazon SageMaker Ground Truth and Amazon Augmented AI work teams fall into one of three [workforce types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management.html): public \(with Amazon Mechanical Turk\), private, and vendor\. To restrict user access to a specific work team using one of these types or the work team ARN, use the `sagemaker:WorkteamType` and/or the `sagemaker:WorkteamArn` condition keys\. For the `sagemaker:WorkteamType` condition key, use [string condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_String)\. For the `sagemaker:WorkteamArn` condition key, use [Amazon Resource Name \(ARN\) condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_ARN)\. If the user attempts to create a labeling job with a restricted work team, SageMaker returns an access denied error\. 

The policies below demonstrate different ways to use the `sagemaker:WorkteamType` and `sagemaker:WorkteamArn` condition keys with appropriate condition operators and valid condition values\. 

The following example uses the `sagemaker:WorkteamType` condition key with the `StringEquals` condition operator to restrict access to a public work team\. It accepts condition values in the following format: `workforcetype-crowd`, where *workforcetype* can equal `public`, `private`, or `vendor`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "RestrictWorkteamType",
            "Effect": "Deny",
            "Action": "sagemaker:CreateLabelingJob",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "sagemaker:WorkteamType": "public-crowd"
                }
            }
        }
    ]
}
```

The following policies show how to restrict access to a public work team using the `sagemaker:WorkteamArn` condition key\. The first shows how to use it with a valid IAM regex\-variant of the work team ARN and the `ArnLike` condition operator\. The second shows how to use it with the `ArnEquals` condition operator and the work team ARN\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "RestrictWorkteamType",
            "Effect": "Deny",
            "Action": "sagemaker:CreateLabelingJob",
            "Resource": "*",
            "Condition": {
                "ArnLike": {
                    "sagemaker:WorkteamArn": "arn:aws:sagemaker:*:*:workteam/public-crowd/*"
                }
            }
        }
    ]
}
```

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "RestrictWorkteamType",
            "Effect": "Deny",
            "Action": "sagemaker:CreateLabelingJob",
            "Resource": "*",
            "Condition": {
                "ArnEquals": {
                    "sagemaker:WorkteamArn": "arn:aws:sagemaker:us-west-2:394669845002:workteam/public-crowd/default"
                }
            }
        }
    ]
}
```

 

### Enforce Encryption of Input Data<a name="sagemaker-condition-kms"></a>

The following policy restricts a user to specify a AWS KMS key to encrypt input data when creating training, hyperparameter tuning, and labeling jobs by using the `sagemaker:VolumeKmsKey` condition key:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EnforceEncryption",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateHyperParameterTuningJob",
                "sagemaker:CreateLabelingJob",
                "sagemaker:CreateFlowDefiniton"
            ],
            "Resource": "*",
            "Condition": {
                "ArnEquals": {
                    "sagemaker:VolumeKmsKey": "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"
                }
            }
        }

     ]
}
```

### Enforce Encryption of Notebook Instance Storage Volume<a name="sagemaker-condition-kms-nbi"></a>

The following policy restricts a user to specify an AWS KMS key to encrypt the attached storage volume when creating or updating a notebook instance by using the `sagemaker:VolumeKmsKey` condition key:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EnforceEncryption",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateNotebookInstance"
            ],
            "Resource": "*",
            "Condition": {
                "ArnLike": {
                    "sagemaker:VolumeKmsKey": "*key/volume-kms-key-12345"
                }
            }
        }

     ]
}
```

### Enforce Network Isolation for Training Jobs<a name="sagemaker-condition-isolation"></a>

The following policy restricts a user to enable network isolation when creating training jobs by using the `sagemaker:NetworkIsolation` condition key:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EnforceIsolation",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateHyperParameterTuningJob"
            ],
            "Resource": "*",
            "Condition": {
                "Bool": {
                    "sagemaker:NetworkIsolation": "true"
                }
            }
        }
    ]
}
```

### Enforce a Specific Instance Type for Training Jobs<a name="sagemaker-condition-instance"></a>

The following policy restricts a user to use a specific instance type when creating training jobs by using the `sagemaker:InstanceTypes` condition key:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EnforceInstanceType",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateHyperParameterTuningJob"
            ],
            "Resource": "*",
            "Condition": {
                "ForAllValues:StringLike": {
                    "sagemaker:InstanceTypes": ["ml.c5.*"]
                }
            }
        }

     ]
}
```

### Enforce a Specific EI Accelerator for Training Jobs<a name="sagemaker-condition-ei"></a>

The following policy restricts a user to use a specific elastic inference \(EI\) accelerator, if an accelerator is provided, when creating or updating notebook instances and when creating endpoint configurations by using the `sagemaker:AcceleratorTypes` condition key:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EnforceAcceleratorType",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateNotebookInstance",
                "sagemaker:UpdateNotebookInstance",
                "sagemaker:CreateEndpointConfig"
            ],
            "Resource": "*",
            "Condition": {
                "ForAllValues:StringEquals": {
                    "sagemaker:AcceleratorTypes": ["ml.eia1.medium"]
                }
            }
        }

     ]
}
```

### Enforce Disabling Internet Access and Root Access for Creating Notebook Instances<a name="sagemaker-condition-nbi-lockdown"></a>

You can disable both internet access and root access to notebook instances to help make them more secure\. For information about controlling root access to a notebook instance, see [Control root access to a SageMaker notebook instance](nbi-root-access.md)\. for information about disabling internet access for a notebook instance, see [Connect a Notebook Instance in a VPC to External Resources](appendix-notebook-and-internet-access.md)\.

The following policy requires a user to disable network access when creating instance, and disable root access when creating or updating a notebook instance\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "LockDownCreateNotebookInstance",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateNotebookInstance"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "sagemaker:DirectInternetAccess": "Disabled",
                    "sagemaker:RootAccess": "Disabled"
                },
                "Null": {
                  "sagemaker:VpcSubnets": "false",
                  "sagemaker:VpcSecurityGroupIds": "false"
                }
            }
        },
        {
            "Sid": "LockDownUpdateNotebookInstance",
            "Effect": "Allow",
            "Action": [
                "sagemaker:UpdateNotebookInstance"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "sagemaker:RootAccess": "Disabled"
                }
            }
        }
     ]
}
```

## Control Access to the SageMaker API by Using Identity\-based Policies<a name="api-access-policy"></a>

To control access to SageMaker API calls and calls to SageMaker hosted endpoints, use identity\-based IAM policies\.

**Topics**
+ [Restrict Access to SageMaker API and Runtime to Calls from Within Your VPC](#api-access-policy-vpc)

### Restrict Access to SageMaker API and Runtime to Calls from Within Your VPC<a name="api-access-policy-vpc"></a>

If you set up an interface endpoint in your VPC, individuals outside the VPC can still connect to the SageMaker API and runtime over the internet unless you attach an IAM policy that restricts access to calls coming from within the VPC to all users and groups that have access to your SageMaker resources\. For information about creating a VPC interface endpoint for the SageMaker API and runtime, see [Connect to SageMaker Through a VPC Interface Endpoint](interface-vpc-endpoint.md)\.

**Important**  
If you apply an IAM policy similar to one of the following, users can't access the specified SageMaker APIs through the console\.

To restrict access to only connections made from within your VPC, create an AWS Identity and Access Management policy that restricts access to only calls that come from within your VPC\. Then add that policy to every AWS Identity and Access Management user, group, or role used to access the SageMaker API or runtime\.

**Note**  
This policy allows connections only to callers within a subnet where you created an interface endpoint\.

```
{
    "Id": "api-example-1",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EnableAPIAccess",
            "Effect": "Allow",
            "Action": [
                "sagemaker:*"
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
            "Sid": "EnableAPIAccess",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedNotebookInstanceUrl"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
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

## Limit Access to SageMaker API and Runtime Calls by IP Address<a name="api-ip-filter"></a>

To allow access to SageMaker API calls and runtime invocations only from IP addresses in a list that you specify, attach an IAM policy that denies access to the API unless the call comes from an IP address in the list to every AWS Identity and Access Management user, group, or role used to access the API or runtime\. For information about creating IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *AWS Identity and Access Management User Guide*\. To specify the list of IP addresses that you want to have access to the API call, use the `IpAddress` condition operator and the `aws:SourceIP` condition context key\. For information about IAM condition operators, see [IAM JSON Policy Elements: Condition Operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html) in the *AWS Identity and Access Management User Guide*\. For information about IAM condition context keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)\.

For example, the following policy allows access to the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) only from IP addresses in the ranges `192.0.2.0`\-`192.0.2.255` and `203.0.113.0`\-`203.0.113.255`:

```
{
    "Version": "2012-10-17",
    "Statement": [

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

## Limit Access to a Notebook Instance by IP Address<a name="nbi-ip-filter"></a>

To allow access to a notebook instance only from IP addresses in a list that you specify, attach an IAM policy that denies access to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreatePresignedNotebookInstanceUrl.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreatePresignedNotebookInstanceUrl.html) unless the call comes from an IP address in the list to every AWS Identity and Access Management user, group, or role used to access the notebook instance\. For information about creating IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *AWS Identity and Access Management User Guide*\. To specify the list of IP addresses that you want to have access to the notebook instance, use the `IpAddress` condition operator and the `aws:SourceIP` condition context key\. For information about IAM condition operators, see [IAM JSON Policy Elements: Condition Operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html) in the *AWS Identity and Access Management User Guide*\. For information about IAM condition context keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)\.

For example, the following policy allows access to a notebook instance only from IP addresses in the ranges `192.0.2.0`\-`192.0.2.255` and `203.0.113.0`\-`203.0.113.255`:

```
{
    "Version": "2012-10-17",
    "Statement": [

        {
            "Effect": "Allow",
            "Action": "sagemaker:CreatePresignedNotebookInstanceUrl",
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

The policy restricts access to both the call to `CreatePresignedNotebookInstanceUrl` and to the URL that the call returns\. The policy also restricts access to opening a notebook instance in the console and is enforced for every HTTP request and WebSocket frame that attempts to connect to the notebook instance\.

**Note**  
Using this method to filter by IP address is incompatible when [connecting to SageMaker through a VPC interface endpoint\.](https://docs.aws.amazon.com/sagemaker/latest/dg/interface-vpc-endpoint.html)\. For information about restricting access to a notebook instance when connecting through a VPC interface endpoint, see [Connect to a Notebook Instance Through a VPC Interface Endpoint](notebook-interface-endpoint.md)\.

## Control Access to SageMaker Resources by Using Tags<a name="access-tag-policy"></a>

Control access to groups of SageMaker resources by attaching tags to the resources and specifying `ResourceTag` conditions in IAM policies\.

**Note**  
Tag\-based policies don't work to restrict the following API calls:  
DeleteImageVersion
DescribeImageVersion
ListAlgorithms
ListCodeRepositories
ListCompilationJobs
ListEndpointConfigs
ListEndpoints
ListFlowDefinitions
ListHumanTaskUis
ListHyperparameterTuningJobs
ListLabelingJobs
ListLabelingJobsForWorkteam
ListModelPackages
ListModels
ListNotebookInstanceLifecycleConfigs
ListNotebookInstances
ListSubscribedWorkteams
ListTags
ListProcessingJobs
ListTrainingJobs
ListTrainingJobsForHyperParameterTuningJob
ListTransformJobs
ListWorkteams
Search

For example, suppose you've defined two different IAM groups, named `DevTeam1` and `DevTeam2`, in your AWS account\. Suppose also that you've created 10 notebook instances, 5 of which are used for one project, and 5 of which are used for a second project\. You want to allow members of `DevTeam1` to make API calls on notebook instances used for the first project, and members of `DevTeam2` to make API calls on notebook instances used for the second project\.

**To control access to API calls \(example\)**

1. Add a tag with the key `Project` and value `A` to the notebook instances used for the first project\. For information about adding tags to SageMaker resources, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AddTags.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AddTags.html)\. 

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
           "sagemaker:AddTags",
           "sagemaker:DeleteTags"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

   For information about creating IAM policies and attaching them to identities, see [Controlling Access Using Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_controlling.html) in the *AWS Identity and Access Management User Guide*\.

1. Create an IAM policy with a `ResourceTag` condition that denies access to the notebook instances used for the first project, and attach that policy to `DevTeam2`\. The following is an example of a policy that denies all API calls on any notebook instance that has a tag with a key of `Project` and a value of `A`:

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
             "sagemaker:ResourceTag/Project": "A"
           }
         }
       },
       {
         "Effect": "Deny",
         "Action": [
           "sagemaker:AddTags",
           "sagemaker:DeleteTags"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

## Require the Presence or Absence of Tags for API Calls<a name="resource-tag"></a>

Require the presence or absence of specific tags or specific tag values by using `RequestTag` condition keys in an IAM policy\. For example, if you want to require that every endpoint created by any member of an IAM group to be created with a tag with the key `environment` and value `dev`, create a policy as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sagemaker:CreateEndpoint",
            "Resource": "arn:aws:sagemaker:*:*:endpoint/*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestTag/environment": "dev"
                }
            }
        }
    ]
}
```