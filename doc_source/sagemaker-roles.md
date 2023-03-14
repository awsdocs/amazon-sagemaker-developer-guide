# SageMaker Roles<a name="sagemaker-roles"></a>

As a managed service, Amazon SageMaker performs operations on your behalf on the AWS hardware that is managed by SageMaker\. SageMaker can perform only operations that the user permits\.

A SageMaker user can grant these permissions with an IAM role \(referred to as an execution role\)\. 

To create and use a locally available execution role, you can use the following procedures\.

## Get execution role<a name="sagemaker-roles-get-execution-role"></a>

You can find the IAM execution role in the following ways:

**From the notebook**

When you run a notebook within SageMaker \(from the SageMaker console or SageMaker Studio\) you can access the execution role with the following code:

```
sagemaker_session = sagemaker.Session()
role = sagemaker.get_execution_role()
```

**Note**  
The execution role is available only when running a notebook within SageMaker\. If you run `get_execution_role` in a notebook not on SageMaker, expect a "region" error\. 

**From the SageMaker console**

Under **Notebook > Notebook instances**, select the notebook\. The ARN is given in the **Permissions and encryption** section\.

## Create execution role<a name="sagemaker-roles-create-execution-role"></a>

Use the following procedure to create an execution role with the IAM managed policy, `AmazonSageMakerFullAccess`, attached\. If your use case requires more granular permissions, use other sections on this page to create an execution role that meets your business needs\.

**Important**  
The IAM managed policy, `AmazonSageMakerFullAccess`, used in the following procedure only grants the execution role permission to perform certain Amazon S3 actions on buckets or objects with `SageMaker`, `Sagemaker`, `sagemaker`, or `aws-glue` in the name\. To learn how to add an additional policy to an execution role to grant it access to other Amazon S3 buckets and objects, see [Add Additional Amazon S3 Permissions to a SageMaker Execution Role](#sagemaker-roles-get-execution-role-s3)\.

**To create a new role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Select **Roles** and then select **Create role**\.

1. Select **SageMaker**\.

1. Select **Next: Permissions**\.

1. The IAM managed policy, `AmazonSageMakerFullAccess` is automatically attached to this role\. To see the permissions included in this policy, select the sideways arrow next to the policy name\. Select **Next: Tags**\.

1. \(Optional\) Add tags and select **Next: Review**\.

1. Give the role a name in the text field under **Role name** and select **Create role**\.

1. On the **Roles** section of the IAM console, select the role you just created\. If needed, use the text box to search for the role using the role name you entered in step 7\.

1. On the role summary page, make note of the ARN\.

With a known ARN for your role, you can programmatically check the role when running the notebook locally or on SageMaker\. Replace `RoleName` with your known ARN:

```
try:
    role = sagemaker.get_execution_role()
except ValueError:
    iam = boto3.client('iam')
    role = iam.get_role(RoleName='AmazonSageMaker-ExecutionRole-20201200T100000')['Role']['Arn']
```

### Add Additional Amazon S3 Permissions to a SageMaker Execution Role<a name="sagemaker-roles-get-execution-role-s3"></a>

When you use a SageMaker feature with resources in Amazon S3, such as input data, the execution role you specify in your request \(for example `CreateTrainingJob`\) is used to access these resources\.

If you attach the IAM managed policy, `AmazonSageMakerFullAccess`, to an execution role, that role has permission to perform certain Amazon S3 actions on buckets or objects with `SageMaker`, `Sagemaker`, `sagemaker`, or `aws-glue` in the name\. It also has permission to perform the following actions on any Amazon S3 resource:

```
"s3:CreateBucket", 
"s3:GetBucketLocation",
"s3:ListBucket",
"s3:ListAllMyBuckets",
"s3:GetBucketCors",
"s3:PutBucketCors"
```

To give an execution role permissions to access one or more specific buckets in Amazon S3, you can attach a policy similar to the following to the role\. This policy grants an IAM role permission to perform all actions that `AmazonSageMakerFullAccess` allows but restricts this access to the buckets *DOC\-EXAMPLE\-BUCKET1* and *DOC\-EXAMPLE\-BUCKET2*\. Refer to the security documentation for the specific SageMaker feature you are using to learn more about the Amazon S3 permissions required for that feature\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:AbortMultipartUpload"
            ],
            "Resource": [
                "arn:aws:s3:::DOC-EXAMPLE-BUCKET1/*",
                "arn:aws:s3:::DOC-EXAMPLE-BUCKET2/*"
            ]
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
                "arn:aws:s3:::DOC-EXAMPLE-BUCKET1",
                "arn:aws:s3:::DOC-EXAMPLE-BUCKET2"
            ]
        }
    ]
}
```

## Passing Roles<a name="sagemaker-roles-pass-role"></a>

Actions like passing a role between services are a common function within SageMaker\. You can find more details on [Actions, Resources, and Condition Keys for SageMaker](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonsagemaker.html#amazonsagemaker-actions-as-permissions) in the *IAM User Guide*\.

You pass the role \(`iam:PassRole`\) when making these API calls: [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateCompilationJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateCompilationJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateImage.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateImage.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateMonitoringSchedule.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateMonitoringSchedule.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html), and [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateImage.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateImage.html)\.

You attach the following trust policy to the IAM role which grants SageMaker principal permissions to assume the role, and is the same for all of the execution roles: 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "sagemaker.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

The permissions that you need to grant to the role vary depending on the API that you call\. The following sections explain these permissions\.

**Note**  
Instead of managing permissions by crafting a permission policy, you can use the AWS\-managed `AmazonSageMakerFullAccess` permission policy\. The permissions in this policy are fairly broad, to allow for any actions you might want to perform in SageMaker\. For a listing of the policy including information about the reasons for adding many of the permissions, see [AWS managed policy: AmazonSageMakerFullAccess](security-iam-awsmanpol.md#security-iam-awsmanpol-AmazonSageMakerFullAccess)\. If you prefer to create custom policies and manage permissions to scope the permissions only to the actions you need to perform with the execution role, see the following topics\.

**Important**  
If you're running into issues, see [Troubleshooting Amazon SageMaker Identity and Access](security_iam_troubleshoot.md)\.

For more information about IAM roles, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

**Topics**
+ [Get execution role](#sagemaker-roles-get-execution-role)
+ [Create execution role](#sagemaker-roles-create-execution-role)
+ [Passing Roles](#sagemaker-roles-pass-role)
+ [CreateAutoMLJob API: Execution Role Permissions](#sagemaker-roles-autopilot-perms)
+ [CreateDomain API: Execution Role Permissions](#sagemaker-roles-createdomain-perms)
+ [CreateImage and UpdateImage APIs: Execution Role Permissions](#sagemaker-roles-createimage-perms)
+ [CreateNotebookInstance API: Execution Role Permissions](#sagemaker-roles-createnotebookinstance-perms)
+ [CreateHyperParameterTuningJob API: Execution Role Permissions](#sagemaker-roles-createhyperparametertiningjob-perms)
+ [CreateProcessingJob API: Execution Role Permissions](#sagemaker-roles-createprocessingjob-perms)
+ [CreateTrainingJob API: Execution Role Permissions](#sagemaker-roles-createtrainingjob-perms)
+ [CreateModel API: Execution Role Permissions](#sagemaker-roles-createmodel-perms)
+ [SageMaker geospatial capabilities roles](sagemaker-geospatial-roles.md)

## CreateAutoMLJob API: Execution Role Permissions<a name="sagemaker-roles-autopilot-perms"></a>

For an execution role that you can pass in a `CreateAutoMLJob` API request, you can attach the following minimum permission policy to the role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
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
        },
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:DescribeEndpointConfig",
                "sagemaker:DescribeModel",
                "sagemaker:InvokeEndpoint",
                "sagemaker:ListTags",
                "sagemaker:DescribeEndpoint",
                "sagemaker:CreateModel",
                "sagemaker:CreateEndpointConfig",
                "sagemaker:CreateEndpoint",
                "sagemaker:DeleteModel",
                "sagemaker:DeleteEndpointConfig",
                "sagemaker:DeleteEndpoint",
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "*"
        }
    ]
}
```

If you specify a private VPC for your AutoML job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
        "ec2:CreateNetworkInterface",
        "ec2:CreateNetworkInterfacePermission",
        "ec2:DeleteNetworkInterface",
        "ec2:DeleteNetworkInterfacePermission",
        "ec2:DescribeNetworkInterfaces",
        "ec2:DescribeVpcs",
        "ec2:DescribeDhcpOptions",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups"
    ]
}
```

If your input is encrypted using server\-side encryption with an AWS KMS–managed key \(SSE\-KMS\), add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
        "kms:Decrypt"
    ]
}
```

If you specify a KMS key in the output configuration of your AutoML job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
    "kms:Encrypt"
    ]
}
```

If you specify a volume KMS key in the resource configuration of your AutoML job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
    "kms:CreateGrant"
    ]
}
```

## CreateDomain API: Execution Role Permissions<a name="sagemaker-roles-createdomain-perms"></a>

The execution role for domains with IAM Identity Center and the user/execution role for IAM domains need the following permissions when you pass an AWS KMS customer managed key as the `KmsKeyId` in the `CreateDomain` API request\. The permissions are enforced during the `CreateApp` API call\.

For an execution role that you can pass in the `CreateDomain` API request, you can attach the following permission policy to the role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:CreateGrant",
                "kms:DescribeKey"
            ],
            "Resource": "arn:aws:kms:region:account-id:key/kms-key-id"
        }
    ]
}
```

Alternatively, if the permissions are specified in a KMS policy, you can attach the following policy to the role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Allow use of the key",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::account-id:role/ExecutionRole"
                ]
            },
            "Action": [
                "kms:CreateGrant",
                "kms:DescribeKey"
            ],
            "Resource": "*"
        }
    ]
}
```

## CreateImage and UpdateImage APIs: Execution Role Permissions<a name="sagemaker-roles-createimage-perms"></a>

For an execution role that you can pass in a `CreateImage` or `UpdateImage` API request, you can attach the following permission policy to the role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchGetImage",
                "ecr:GetDownloadUrlForLayer"
            ],
            "Resource": "*"
        },
        {
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

## CreateNotebookInstance API: Execution Role Permissions<a name="sagemaker-roles-createnotebookinstance-perms"></a>

The permissions that you grant to the execution role for calling the `CreateNotebookInstance` API depend on what you plan to do with the notebook instance\. If you plan to use it to invoke SageMaker APIs and pass the same role when calling the `CreateTrainingJob` and `CreateModel` APIs, attach the following permissions policy to the role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:*",
                "ecr:GetAuthorizationToken",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "ecr:BatchCheckLayerAvailability",
                "ecr:SetRepositoryPolicy",
                "ecr:CompleteLayerUpload",
                "ecr:BatchDeleteImage",
                "ecr:UploadLayerPart",
                "ecr:DeleteRepositoryPolicy",
                "ecr:InitiateLayerUpload",
                "ecr:DeleteRepository",
                "ecr:PutImage",
                "ecr:CreateRepository",
                "cloudwatch:PutMetricData",
                "cloudwatch:GetMetricData",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:ListMetrics",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents",
                "logs:GetLogEvents",
                "s3:CreateBucket",
                "s3:ListBucket",
                "s3:GetBucketLocation",
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "robomaker:CreateSimulationApplication",
                "robomaker:DescribeSimulationApplication",
                "robomaker:DeleteSimulationApplication",
                "robomaker:CreateSimulationJob",
                "robomaker:DescribeSimulationJob",
                "robomaker:CancelSimulationJob",
                "ec2:CreateVpcEndpoint",
                "ec2:DescribeRouteTables",
                "elasticfilesystem:DescribeMountTargets"
            ],
            "Resource": "*"
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

To tighten the permissions, limit them to specific Amazon S3 and Amazon ECR resources, by restricting `"Resource": "*"`, as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:*",
                "ecr:GetAuthorizationToken",
                "cloudwatch:PutMetricData",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents",
                "logs:GetLogEvents"
            ],
            "Resource": "*"
        },
        {
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
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::inputbucket"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::inputbucket/object1",
                "arn:aws:s3:::outputbucket/path",
                "arn:aws:s3:::inputbucket/object2",
                "arn:aws:s3:::inputbucket/object3"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": [
                "arn:aws:ecr:region::repository/my-repo1",
                "arn:aws:ecr:region::repository/my-repo2",
                "arn:aws:ecr:region::repository/my-repo3"
            ]
        }
    ]
}
```

If you plan to access other resources, such as Amazon DynamoDB or Amazon Relational Database Service, add the relevant permissions to this policy\.

In the preceding policy, you scope the policy as follows:
+ Scope the `s3:ListBucket` permission to the specific bucket that you specify as `InputDataConfig.DataSource.S3DataSource.S3Uri` in a `CreateTrainingJob` request\.
+ Scope `s3:GetObject `, `s3:PutObject`, and `s3:DeleteObject` permissions as follows:
  + Scope to the following values that you specify in a `CreateTrainingJob` request:

    `InputDataConfig.DataSource.S3DataSource.S3Uri`

    `OutputDataConfig.S3OutputPath`
  + Scope to the following values that you specify in a `CreateModel` request:

    `PrimaryContainer.ModelDataUrl`

    `SuplementalContainers.ModelDataUrl`
+ Scope `ecr` permissions as follows:
  + Scope to the `AlgorithmSpecification.TrainingImage` value that you specify in a `CreateTrainingJob` request\.
  + Scope to the `PrimaryContainer.Image` value that you specify in a `CreateModel` request:

The `cloudwatch` and `logs` actions are applicable for "\*" resources\. For more information, see [CloudWatch Resources and Operations](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/iam-access-control-overview-cw.html#CloudWatch_ARN_Format) in the Amazon CloudWatch User Guide\.



## CreateHyperParameterTuningJob API: Execution Role Permissions<a name="sagemaker-roles-createhyperparametertiningjob-perms"></a>

For an execution role that you can pass in a `CreateHyperParameterTuningJob` API request, you can attach the following permission policy to the role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "*"
        }
    ]
}
```

Instead of the specifying `"Resource": "*"`, you could scope these permissions to specific Amazon S3 and Amazon ECR resources:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "ecr:GetAuthorizationToken"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::inputbucket"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::inputbucket/object",
                "arn:aws:s3:::outputbucket/path"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "arn:aws:ecr:region::repository/my-repo"
        }
    ]
}
```

If the training container associated with the hyperparameter tuning job needs to access other data sources, such as DynamoDB or Amazon RDS resources, add relevant permissions to this policy\.

In the preceding policy, you scope the policy as follows:
+ Scope the `s3:ListBucket` permission to a specific bucket that you specify as the `InputDataConfig.DataSource.S3DataSource.S3Uri` in a `CreateTrainingJob` request\.
+ Scope the `s3:GetObject `and `s3:PutObject` permissions to the following objects that you specify in the input and output data configuration in a `CreateHyperParameterTuningJob` request:

  `InputDataConfig.DataSource.S3DataSource.S3Uri`

  `OutputDataConfig.S3OutputPath`
+ Scope Amazon ECR permissions to the registry path \(`AlgorithmSpecification.TrainingImage`\) that you specify in a `CreateHyperParameterTuningJob` request\.

The `cloudwatch` and `logs` actions are applicable for "\*" resources\. For more information, see [CloudWatch Resources and Operations](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/iam-access-control-overview-cw.html#CloudWatch_ARN_Format) in the Amazon CloudWatch User Guide\.

If you specify a private VPC for your hyperparameter tuning job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
        "ec2:CreateNetworkInterface",
        "ec2:CreateNetworkInterfacePermission",
        "ec2:DeleteNetworkInterface",
        "ec2:DeleteNetworkInterfacePermission",
        "ec2:DescribeNetworkInterfaces",
        "ec2:DescribeVpcs",
        "ec2:DescribeDhcpOptions",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups"
    ]
}
```

If your input is encrypted using server\-side encryption with an AWS KMS–managed key \(SSE\-KMS\), add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
        "kms:Decrypt"
    ]
}
```

If you specify a KMS key in the output configuration of your hyperparameter tuning job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
    "kms:Encrypt"
    ]
}
```

If you specify a volume KMS key in the resource configuration of your hyperparameter tuning job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
    "kms:CreateGrant"
    ]
}
```

## CreateProcessingJob API: Execution Role Permissions<a name="sagemaker-roles-createprocessingjob-perms"></a>

For an execution role that you can pass in a `CreateProcessingJob` API request, you can attach the following permission policy to the role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "*"
        }
    ]
}
```

Instead of the specifying `"Resource": "*"`, you could scope these permissions to specific Amazon S3 and Amazon ECR resources:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "ecr:GetAuthorizationToken"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::inputbucket"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::inputbucket/object",
                "arn:aws:s3:::outputbucket/path"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "arn:aws:ecr:region::repository/my-repo"
        }
    ]
}
```

If `CreateProcessingJob.AppSpecification.ImageUri` needs to access other data sources, such as DynamoDB or Amazon RDS resources, add relevant permissions to this policy\.

In the preceding policy, you scope the policy as follows:
+ Scope the `s3:ListBucket` permission to a specific bucket that you specify as the `ProcessingInputs` in a `CreateProcessingJob` request\.
+ Scope the `s3:GetObject `and `s3:PutObject` permissions to the objects that will be downloaded or uploaded in the `ProcessingInputs` and `ProcessingOutputConfig` in a `CreateProcessingJob` request\.
+ Scope Amazon ECR permissions to the registry path \(`AppSpecification.ImageUri`\) that you specify in a `CreateProcessingJob` request\.

The `cloudwatch` and `logs` actions are applicable for "\*" resources\. For more information, see [CloudWatch Resources and Operations](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/iam-access-control-overview-cw.html#CloudWatch_ARN_Format) in the Amazon CloudWatch User Guide\.

If you specify a private VPC for your processing job, add the following permissions\. Don't scope in the policy with any conditions or resource filters\. Otherwise, the validation checks that occur during the creation of the processing job fail\.

```
{
    "Effect": "Allow",
    "Action": [
        "ec2:CreateNetworkInterface",
        "ec2:CreateNetworkInterfacePermission",
        "ec2:DeleteNetworkInterface",
        "ec2:DeleteNetworkInterfacePermission",
        "ec2:DescribeNetworkInterfaces",
        "ec2:DescribeVpcs",
        "ec2:DescribeDhcpOptions",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups"
    ]
}
```

If your input is encrypted using server\-side encryption with an AWS KMS–managed key \(SSE\-KMS\), add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
        "kms:Decrypt"
    ]
}
```

If you specify a KMS key in the output configuration of your processing job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
    "kms:Encrypt"
    ]
}
```

If you specify a volume KMS key in the resource configuration of your processing job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
    "kms:CreateGrant"
    ]
}
```

## CreateTrainingJob API: Execution Role Permissions<a name="sagemaker-roles-createtrainingjob-perms"></a>

For an execution role that you can pass in a `CreateTrainingJob` API request, you can attach the following permission policy to the role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "*"
        }
    ]
}
```

Instead of the specifying `"Resource": "*"`, you could scope these permissions to specific Amazon S3 and Amazon ECR resources:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "ecr:GetAuthorizationToken"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::inputbucket"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::inputbucket/object",
                "arn:aws:s3:::outputbucket/path"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "arn:aws:ecr:region::repository/my-repo"
        }
    ]
}
```

If `CreateTrainingJob.AlgorithSpecifications.TrainingImage` needs to access other data sources, such as DynamoDB or Amazon RDS resources, add relevant permissions to this policy\.

In the preceding policy, you scope the policy as follows:
+ Scope the `s3:ListBucket` permission to a specific bucket that you specify as the `InputDataConfig.DataSource.S3DataSource.S3Uri` in a `CreateTrainingJob` request\.
+ Scope the `s3:GetObject `and `s3:PutObject` permissions to the following objects that you specify in the input and output data configuration in a `CreateTrainingJob` request:

  `InputDataConfig.DataSource.S3DataSource.S3Uri`

  `OutputDataConfig.S3OutputPath`
+ Scope Amazon ECR permissions to the registry path \(`AlgorithmSpecification.TrainingImage`\) that you specify in a `CreateTrainingJob` request\.

The `cloudwatch` and `logs` actions are applicable for "\*" resources\. For more information, see [CloudWatch Resources and Operations](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/iam-access-control-overview-cw.html#CloudWatch_ARN_Format) in the Amazon CloudWatch User Guide\.

If you specify a private VPC for your training job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
      "ec2:CreateNetworkInterface",
      "ec2:CreateNetworkInterfacePermission",
      "ec2:DeleteNetworkInterface",
      "ec2:DeleteNetworkInterfacePermission",
      "ec2:DescribeNetworkInterfaces",
      "ec2:DescribeVpcs",
      "ec2:DescribeDhcpOptions",
      "ec2:DescribeSubnets",
      "ec2:DescribeSecurityGroups"
    ]
}
```

If your input is encrypted using server\-side encryption with an AWS KMS–managed key \(SSE\-KMS\), add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
        "kms:Decrypt"
    ]
}
```

If you specify a KMS key in the output configuration of your training job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
    "kms:Encrypt"
    ]
}
```

If you specify a volume KMS key in the resource configuration of your training job, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
    "kms:CreateGrant"
    ]
}
```

## CreateModel API: Execution Role Permissions<a name="sagemaker-roles-createmodel-perms"></a>

For an execution role that you can pass in a `CreateModel` API request, you can attach the following permission policy to the role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "s3:GetObject",
                "s3:ListBucket",
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "*"
        }
    ]
}
```

Instead of the specifying `"Resource": "*"`, you can scope these permissions to specific Amazon S3 and Amazon ECR resources:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "ecr:GetAuthorizationToken"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::inputbucket/object"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": [
                "arn:aws:ecr:region::repository/my-repo",
                "arn:aws:ecr:region::repository/my-repo"
             ]
        }
    ]
}
```

If `CreateModel.PrimaryContainer.Image` need to access other data sources, such as Amazon DynamoDB or Amazon RDS resources, add relevant permissions to this policy\.

In the preceding policy, you scope the policy as follows:
+ Scope S3 permissions to objects that you specify in the `PrimaryContainer.ModelDataUrl` in a [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) request\.
+ Scope Amazon ECR permissions to a specific registry path that you specify as the `PrimaryContainer.Image` and `SecondaryContainer.Image` in a `CreateModel` request\.

The `cloudwatch` and `logs` actions are applicable for "\*" resources\. For more information, see [CloudWatch Resources and Operations](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/iam-access-control-overview-cw.html#CloudWatch_ARN_Format) in the Amazon CloudWatch User Guide\.

**Note**  
If you plan to use the [SageMaker deployment guardrails feature](https://docs.aws.amazon.com/sagemaker/latest/dg/deployment-guardrails.html) for model deployment in production, ensure that your execution role has permission to perform the `cloudwatch:DescribeAlarms` action on your auto\-rollback alarms\.

If you specify a private VPC for your model, add the following permissions:

```
{
    "Effect": "Allow",
    "Action": [
        "ec2:CreateNetworkInterface",
        "ec2:CreateNetworkInterfacePermission",
        "ec2:DeleteNetworkInterface",
        "ec2:DeleteNetworkInterfacePermission",
        "ec2:DescribeNetworkInterfaces",
        "ec2:DescribeVpcs",
        "ec2:DescribeDhcpOptions",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups"
    ]
}
```