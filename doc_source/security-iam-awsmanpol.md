# AWS Managed Policies for Amazon SageMaker<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) to which the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the `ReadOnlyAccess` AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

**Important**  
We recommend that you use the most restricted policy that allows you to perform your use case\.

The following AWS managed policies, which you can attach to users in your account, are specific to Amazon SageMaker:
+ **`AmazonSageMakerFullAccess`** – Grants full access to Amazon SageMaker resources and the supported operations\. This does not provide unrestricted Amazon S3 access, but supports buckets and objects with specific `sagemaker` tags\. This policy allows all IAM roles to be passed to Amazon SageMaker, but only allows IAM roles with "AmazonSageMaker" in them to be passed to the AWS Glue, AWS Step Functions, and AWS RoboMaker services\.
+ **`AmazonSageMakerReadOnly`** – Grants read\-only access to Amazon SageMaker resources\. 

The following AWS managed policies can be attached to users in your account but are not recommended:
+ [http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_administrator](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_administrator) – Grants all actions for all AWS services and for all resources in the account\. 
+ [http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_data-scientist](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#jf_data-scientist) – Grants a wide range of permissions to cover most of the use cases \(primarily for analytics and business intelligence\) encountered by data scientists\.

You can review these permissions policies by signing in to the IAM console and searching for them\.

You can also create your own custom IAM policies to allow permissions for Amazon SageMaker actions and resources as you need them\. You can attach these custom policies to the IAM users or groups that require them\. 

**Topics**
+ [AWS managed policy: AmazonSageMakerFullAccess](#security-iam-awsmanpol-AmazonSageMakerFullAccess)
+ [AWS managed policy: AmazonSageMakerReadOnly](#security-iam-awsmanpol-AmazonSageMakerReadOnly)
+ [AWS managed policies for Amazon SageMaker Canvas](security-iam-awsmanpol-canvas.md)
+ [AWS Managed Policies for Amazon SageMaker Ground Truth](security-iam-awsmanpol-ground-truth.md)
+ [AWS Managed Policies for SageMaker Pipelines](security-iam-awsmanpol-pipelines.md)
+ [AWS Managed Policies for SageMaker projects and JumpStart](security-iam-awsmanpol-sc.md)
+ [SageMaker Updates to AWS Managed Policies](#security-iam-awsmanpol-updates)

## AWS managed policy: AmazonSageMakerFullAccess<a name="security-iam-awsmanpol-AmazonSageMakerFullAccess"></a>

This policy grants administrative permissions that allow a principal full access to all Amazon SageMaker resources and operations\. The policy also provides select access to related services\. This policy allows all IAM roles to be passed to Amazon SageMaker, but only allows IAM roles with "AmazonSageMaker" in them to be passed to the AWS Glue, AWS Step Functions, and AWS RoboMaker services\. This policy does not include permissions to create an Amazon SageMaker domain\. For information on the policy needed to create a domain, see [Create an IAM Administrator User and Group ](gs-set-up.md#gs-account-user)\.

**Permissions details**

This policy includes the following permissions\.
+ `application-autoscaling` – Allows principals to automatically scale a SageMaker real\-time inference endpoint\.
+ `athena` – Allows principals to query a list of data catalogs, databases, and table metadata from Amazon Athena\.
+ `aws-marketplace` – Allows principals to view AWS AI Marketplace subscriptions\. You need this if you want to access SageMaker software subscribed in AWS Marketplace\.
+ `cloudformation` – Allows principals to get AWS CloudFormation templates for using SageMaker JumpStart solutions and Pipelines\. SageMaker JumpStart creates resources necessary to run end\-to\-end machine learning solutions that tie SageMaker to other AWS services\. SageMaker Pipelines creates new projects that are backed by AWS Service Catalog\.
+ `cloudwatch` – Allows principals to post CloudWatch metrics, interact with alarms, and upload logs to CloudWatch Logs in your account\.
+ `codebuild` – Allows principals to store AWS CodeBuild artifacts for SageMaker Pipeline and Projects\.
+ `codecommit` – Needed for AWS CodeCommit integration with SageMaker notebook instances\.
+ `cognito-idp` – Needed for Amazon SageMaker Ground Truth to define private workforce and work teams\.
+ `ec2` – Needed for SageMaker to manage Amazon EC2 resources and network interfaces when you specify an Amazon VPC for your SageMaker jobs, models, endpoints, and notebook instances\.
+ `ecr` – Needed to pull and store Docker artifacts for Amazon SageMaker Studio \(custom images\), training, processing, batch inference, and inference endpoints\. This is also required to use your own container in SageMaker\. Additional permissions for SageMaker JumpStart solutions are required to create and remove custom images on behalf of users\.
+ `elastic-inference` – Allows principals to connect to Amazon Elastic Inference for using SageMaker notebook instances and endpoints\.
+ `elasticfilesystem` – Allows principals to access Amazon Elastic File System\. This is needed for SageMaker to use data sources in Amazon Elastic File System for training machine learning models\.
+ `fsx` – Allows principals to access Amazon FSx\. This is needed for SageMaker to use data sources in Amazon FSx for training machine learning models\.
+ `glue` – Needed for inference pipeline pre\-processing from within SageMaker notebook instances\.
+ `groundtruthlabeling` – Needed for Ground Truth labeling jobs\. The `groundtruthlabeling` endpoint is accessed by the Ground Truth console\.
+ `iam` – Needed to give the SageMaker console access to available IAM roles and create service\-linked roles\.
+ `kms` – Needed to give the SageMaker console access to available AWS KMS keys and retrieve them for any specified AWS KMS aliases in jobs and endpoints\.
+ `lambda` – Allows principals to invoke and get a list of AWS Lambda functions\.
+ `logs` – Needed to allow SageMaker jobs and endpoints to publish log streams\.
+ `redshift` – Allows principals to access Amazon Redshift cluster credentials\.
+ `redshift-data` – Allows principals to use data from Amazon Redshift to run, describe, and cancel statements; get statement results; and list schemas and tables\.
+ `robomaker` – Allows principals to have full access to create, get descriptions, and delete AWS RoboMaker simulation applications and jobs\. This is also needed to run reinforcement learning examples on notebook instances\.
+ `s3` – Allows principals to have full access to Amazon S3 resources pertaining to SageMaker, but not all of Amazon S3\.
+ `sagemaker` – Allows principals to list tags on Amazon SageMaker user profiles\.
+ `secretsmanager` – Allows principals to have full access to AWS Secrets Manager\. The principals can securely encrypt, store, and retrieve credentials for databases and other services\. This is also needed for SageMaker notebook instances with SageMaker code repositories that use GitHub\.
+ `servicecatalog` – Allows principals to use AWS Service Catalog\. The principals can create, get a list of, update, or terminate provisioned products, such as servers, databases, websites, or applications deployed using AWS resources\. This is needed for SageMaker JumpStart and Projects to find and read service catalog products and launch AWS resources in user accounts\.
+ `sns` – Allows principals to get a list of Amazon SNS topics\. This is needed for endpoints with Async Inference enabled for notifying users that their inference has completed\.
+ `states` – Needed for SageMaker JumpStart and Pipelines to use a service catalog to create step function resources\.
+ `tag` – Needed for SageMaker Pipelines to render in Studio\. Studio needs resources tagged with particular `sagemaker:project-id` tag\-key\. This requires the `tag:GetResources` permission\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:*"
            ],
            "NotResource": [
                "arn:aws:sagemaker:*:*:domain/*",
                "arn:aws:sagemaker:*:*:user-profile/*",
                "arn:aws:sagemaker:*:*:app/*",
                "arn:aws:sagemaker:*:*:flow-definition/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedDomainUrl",
                "sagemaker:DescribeDomain",
                "sagemaker:ListDomains",
                "sagemaker:DescribeUserProfile",
                "sagemaker:ListUserProfiles",
                "sagemaker:*App",
                "sagemaker:ListApps"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "sagemaker:*",
            "Resource": [
                "arn:aws:sagemaker:*:*:flow-definition/*"
            ],
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
                "cloudformation:GetTemplateSummary",
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
                "codecommit:List*",
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
                "cognito-idp:List*",
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
                "ecr:StartImageScan",
                "elastic-inference:Connect",
                "elasticfilesystem:DescribeFileSystems",
                "elasticfilesystem:DescribeMountTargets",
                "fsx:DescribeFileSystems",
                "glue:CreateJob",
                "glue:DeleteJob",
                "glue:GetJob*",
                "glue:GetTable*",
                "glue:GetWorkflowRun",
                "glue:ResetJobBookmark",
                "glue:StartJobRun",
                "glue:StartWorkflowRun",
                "glue:UpdateJob",
                "groundtruthlabeling:*",
                "iam:ListRoles",
                "kms:DescribeKey",
                "kms:ListAliases",
                "lambda:ListFunctions",
                "logs:CreateLogDelivery",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DeleteLogDelivery",
                "logs:Describe*",
                "logs:GetLogDelivery",
                "logs:GetLogEvents",
                "logs:ListLogDeliveries",
                "logs:PutLogEvents",
                "logs:PutResourcePolicy",
                "logs:UpdateLogDelivery",
                "robomaker:CreateSimulationApplication",
                "robomaker:DescribeSimulationApplication",
                "robomaker:DeleteSimulationApplication",
                "robomaker:CreateSimulationJob",
                "robomaker:DescribeSimulationJob",
                "robomaker:CancelSimulationJob",
                "secretsmanager:ListSecrets",
                "servicecatalog:Describe*",
                "servicecatalog:List*",
                "servicecatalog:ScanProvisionedProducts",
                "servicecatalog:SearchProducts",
                "servicecatalog:SearchProvisionedProducts",
                "sns:ListTopics",
                "tag:GetResources"
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
            "Resource": [
                "arn:aws:ecr:*:*:repository/*sagemaker*"
            ]
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
            "Action": [
                "codebuild:BatchGetBuilds",
                "codebuild:StartBuild"
            ],
            "Resource": [
                "arn:aws:codebuild:*:*:project/sagemaker*",
                "arn:aws:codebuild:*:*:build/*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "states:DescribeExecution",
                "states:GetExecutionHistory",
                "states:StartExecution",
                "states:StopExecution",
                "states:UpdateStateMachine"
            ],
            "Resource": [
                "arn:aws:states:*:*:statemachine:*sagemaker*",
                "arn:aws:states:*:*:execution:*sagemaker*:*"
            ],
            "Effect": "Allow"
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
                "servicecatalog:ProvisionProduct"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "servicecatalog:TerminateProvisionedProduct",
                "servicecatalog:UpdateProvisionedProduct"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "servicecatalog:userLevel": "self"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:AbortMultipartUpload"
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
                "s3:GetObject"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "s3:ExistingObjectTag/servicecatalog:provisioning": "true"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:ListAllMyBuckets",
                "s3:GetBucketCors",
                "s3:PutBucketCors"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetBucketAcl",
                "s3:PutObjectAcl"
            ],
            "Resource": [
                "arn:aws:s3:::*SageMaker*",
                "arn:aws:s3:::*Sagemaker*",
                "arn:aws:s3:::*sagemaker*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": [
                "arn:aws:lambda:*:*:function:*sagemaker*",
                "arn:aws:lambda:*:*:function:*SageMaker*",
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
                "sns:CreateTopic",
                "sns:Publish"
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
            "Resource": "arn:aws:iam::*:role/*AmazonSageMaker*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "glue.amazonaws.com",
                        "robomaker.amazonaws.com",
                        "states.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::*:role/*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "sagemaker.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "athena:ListDataCatalogs",
                "athena:ListDatabases",
                "athena:ListTableMetadata",
                "athena:GetQueryExecution",
                "athena:GetQueryResults",
                "athena:StartQueryExecution",
                "athena:StopQueryExecution"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:CreateTable"
            ],
            "Resource": [
                "arn:aws:glue:*:*:table/*/sagemaker_tmp_*",
                "arn:aws:glue:*:*:table/sagemaker_featurestore/*",
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:UpdateTable"
              ],
              "Resource": [
                "arn:aws:glue:*:*:table/sagemaker_featurestore/*",
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/sagemaker_featurestore"
              ]
            },
            {
              "Effect": "Allow",
              "Action": [
                "glue:DeleteTable"
            ],
            "Resource": [
                "arn:aws:glue:*:*:table/*/sagemaker_tmp_*",
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:GetDatabases",
                "glue:GetTable",
                "glue:GetTables"
            ],
            "Resource": [
                "arn:aws:glue:*:*:table/*",
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:CreateDatabase",
                "glue:GetDatabase"
            ],
            "Resource": [
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/sagemaker_featurestore",
                "arn:aws:glue:*:*:database/sagemaker_processing",
                "arn:aws:glue:*:*:database/default",
                "arn:aws:glue:*:*:database/sagemaker_data_wrangler"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "redshift-data:ExecuteStatement",
                "redshift-data:DescribeStatement",
                "redshift-data:CancelStatement",
                "redshift-data:GetStatementResult",
                "redshift-data:ListSchemas",
                "redshift-data:ListTables"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "redshift:GetClusterCredentials"
            ],
            "Resource": [
                "arn:aws:redshift:*:*:dbuser:*/sagemaker_access*",
                "arn:aws:redshift:*:*:dbname:*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:ListStackResources"
            ],
            "Resource": "arn:aws:cloudformation:*:*:stack/SC-*"
        }
    ]
}
```

## AWS managed policy: AmazonSageMakerReadOnly<a name="security-iam-awsmanpol-AmazonSageMakerReadOnly"></a>

This policy grants read\-only access to Amazon SageMaker through the AWS Management Console and SDK\.

**Permissions details**

This policy includes the following permissions\.
+ `application-autoscaling` – Allows users to browse descriptions of scalable SageMaker real\-time inference endpoints\.
+ `aws-marketplace` – Allows users to view AWS AI Marketplace subscriptions\.
+ `cloudwatch` – Allows users to receive CloudWatch alarms\.
+ `cognito-idp` – Needed for Amazon SageMaker Ground Truth to browse descriptions and lists of private workforce and work teams\.
+ `ecr` – Needed to read Docker artifacts for training and inference\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:Describe*",
                "sagemaker:List*",
                "sagemaker:BatchGetMetrics",
                "sagemaker:GetDeviceRegistration",
                "sagemaker:GetDeviceFleetReport",
                "sagemaker:GetSearchSuggestions",
                "sagemaker:BatchGetRecord",
                "sagemaker:GetRecord",
                "sagemaker:Search",
                "sagemaker:QueryLineage",
                "sagemaker:GetLineageGroupPolicy",
                "sagemaker:BatchDescribeModelPackage",
                "sagemaker:GetModelPackageGroupPolicy"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "application-autoscaling:DescribeScalableTargets",
                "application-autoscaling:DescribeScalingActivities",
                "application-autoscaling:DescribeScalingPolicies",
                "application-autoscaling:DescribeScheduledActions",
                "aws-marketplace:ViewSubscriptions",
                "cloudwatch:DescribeAlarms",
                "cognito-idp:DescribeUserPool",
                "cognito-idp:DescribeUserPoolClient",
                "cognito-idp:ListGroups",
                "cognito-idp:ListIdentityProviders",
                "cognito-idp:ListUserPoolClients",
                "cognito-idp:ListUserPools",
                "cognito-idp:ListUsers",
                "cognito-idp:ListUsersInGroup",
                "ecr:Describe*"
            ],
            "Resource": "*"
        }
    ]
}
```

## SageMaker Updates to AWS Managed Policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for SageMaker since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the [SageMaker document history](https://docs.aws.amazon.com/sagemaker/latest/dg/doc-history.html) page\.


| Policy | Version | Change | Date | 
| --- | --- | --- | --- | 
| [AmazonSageMakerFullAccess](#security-iam-awsmanpol-AmazonSageMakerFullAccess) | 23 |  Add `glue:UpdateTable`\.  | June 29, 2022 | 
| AmazonSageMakerFullAccess | 22 |  Add `cloudformation:ListStackResources`\.  | May 1, 2022 | 
| [AmazonSageMakerReadOnly](#security-iam-awsmanpol-AmazonSageMakerReadOnly) | 11 |  Add `sagemaker:QueryLineage`, `sagemaker:GetLineageGroupPolicy`, `sagemaker:BatchDescribeModelPackage`, `sagemaker:GetModelPackageGroupPolicy` permissions\.  | December 1, 2021 | 
| AmazonSageMakerFullAccess | 21 |  Add `sns:Publish` permissions for endpoints with Async Inference enabled\.  | September 8, 2021 | 
| AmazonSageMakerFullAccess | 20 |  Update `iam:PassRole` resources and permissions\.  |  July 15, 2021  | 
| AmazonSageMakerReadOnly | 10 |  New API `BatchGetRecord` added for SageMaker Feature Store\.   | June 10, 2021 | 
|  |  |  SageMaker started tracking changes for its AWS managed policies\.  | June 1, 2021 | 