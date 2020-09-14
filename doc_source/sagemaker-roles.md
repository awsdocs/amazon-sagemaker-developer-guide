# SageMaker Roles<a name="sagemaker-roles"></a>

As a managed service, SageMaker performs operations on your behalf on the AWS hardware that is managed by SageMaker\. SageMaker can perform only operations that the user permits\.

A SageMaker user can grant these permissions with an IAM role \(referred to as an execution role\)\. 

## Get execution role<a name="sagemaker-roles-get-execution-role"></a>

When you run a notebook within SageMaker you can access the execution role with the following code:

```
sagemaker_session = sagemaker.Session()
role = sagemaker.get_execution_role()
```

**Note**  
The execution role is intended to be only available when running a notebook within SageMaker\. If you run `get_execution_role` in a notebook not on SageMaker, you will get a "region" error\. 

To use a locally available execution role, you can use the following procedures\. Check the IAM role ARN that your created when you created your the Notebook Instance or Studio application\. This can be found in the console in the detail page under Permissions and Encryption\.

**To create a new role**

1. Log onto the console \-> IAM \-> Roles \-> Create Role

1. Create a service\-linked role with `sagemaker.amazonaws.com`

1. Give the role `AmazonSageMakerFullAccess`

1. Give the role `AmazonS3FullAccess` \(limit the permissions to specific buckets if possible\)

1. Make note of the ARN once it is created

With a known ARN for your role, you can programatically check the role when running the notebook locally or on SageMaker\. Replace `RoleName` with your known ARN:

```
try:
    role = sagemaker.get_execution_role()
except ValueError:
    iam = boto3.client('iam')
    role = iam.get_role(RoleName='AmazonSageMaker-ExecutionRole-20201200T100000')['Role']['Arn']
```

## Passing Roles<a name="sagemaker-roles-pass-role"></a>

Actions like passing a role between services are a common function within SageMaker\. You can find more details on [Actions, Resources, and Condition Keys for SageMaker](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonsagemaker.html#amazonsagemaker-actions-as-permissions) in the *IAM User Guide*\.

You pass the role \(`iam:PassRole`\) when making these API calls: [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html), [ `CreateCompilationJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateCompilationJob.html), [ `CreateDomain`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html), [ `CreateFlowDefiniton`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html), [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateMonitoringSchedule.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateMonitoringSchedule.html), [ `CreateNotebookInstance`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html), and [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html)\.

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
Instead of managing permissions by crafting a permission policy, you can use the AWS\-managed `AmazonSageMakerFullAccess` permission policy\. The permissions in this policy are fairly broad, to allow for any actions you might want to perform in SageMaker\. For a listing of the policy including information about the reasons for adding many of the permissions, see [AmazonSageMakerFullAccess Policy](#sagemaker-roles-amazonsagemakerfullaccess-policy)\. If you prefer to create custom policies and manage permissions to scope the permissions only to the actions you need to perform with the execution role, see the following topics\.

For more information about IAM roles, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

**Topics**

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
                "fsx:DescribeFileSystem",
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
                "arn:aws:ecr:::repository/my-repo1",
                "arn:aws:ecr:::repository/my-repo2",
                "arn:aws:ecr:::repository/my-repo3"
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
            "Resource": "arn:aws:ecr:::repository/my-repo"
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
            "Resource": "arn:aws:ecr:::repository/my-repo"
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

If you specify a private VPC for your processing job, add the following permissions:

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
            "Resource": "arn:aws:ecr:::repository/my-repo"
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
                "arn:aws:s3:::inputbucket/object",
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
                "arn:aws:ecr:::repository/my-repo",
                "arn:aws:ecr:::repository/my-repo"
             ]
        }
    ]
}
```

If `CreateModel.PrimaryContainer.Image` need to access other data sources, such as Amazon DynamoDB or Amazon RDS resources, add relevant permissions to this policy\.

In the preceding policy, you scope the policy as follows:
+ Scope S3 permissions to objects that you specify in the `PrimaryContainer.ModelDataUrl` in a [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) request\.
+ Scope Amazon ECR permissions to a specific registry path that you specify as the `PrimaryContainer.Image` and `SecondaryContainer.Image` in a `CreateModel` request\.

The `cloudwatch` and `logs` actions are applicable for "\*" resources\. For more information, see [CloudWatch Resources and Operations](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/iam-access-control-overview-cw.html#CloudWatch_ARN_Format) in the Amazon CloudWatch User Guide\.

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
```

## AmazonSageMakerFullAccess Policy<a name="sagemaker-roles-amazonsagemakerfullaccess-policy"></a>

The [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) managed policy includes all of the necessary permissions to perform most actions in SageMaker\. You can use attach this policy to any role that you pass to a SageMaker execution role\. You can also create more narrowly\-scoped policies if you want more granular control of the permissions that you grant to your execution role\.

The following list explains why some of the categories of permissions in the `AmazonSageMakerFullAccess` policy are needed\.

`application-autoscaling`  
Needed for automatically scaling a SageMaker real\-time inference endpoint\.

`aws-marketplace`  
Needed to view AWS AI Marketplace subscriptions\.

`cloudwatch`  
Needed to post CloudWatch metrics, interact with alarms, and upload CloudWatch Logs logs in your account\.

`codecommit`  
Needed for AWS CodeCommit integration with SageMaker notebook instances\.

`cognito`  
Needed for SageMaker Ground Truth to define your private workforce and work teams\.

`ec2`  
Needed to manage elastic network interfaces when you specify a Amazon VPC for your SageMaker jobs and notebook instances\.

`ec2:DescribeVpcs`  
All SageMaker services launch Amazon EC2 instances and require this permission set\.

`ecr`  
Needed to pull and store Docker artifacts for training and inference\. This is required only if you use your own container in SageMaker\.

`elastic-inference`  
Needed to integrate Amazon Elastic Inference with SageMaker\.

`glue`  
Needed for inference pipeline pre\-processing from within SageMaker notebook instances\.

`groundtruthlabeling`  
Needed for SageMaker Ground Truth\.

`iam:ListRoles`  
Needed to give the SageMaker console access to list available roles\.

`kms`  
Needed to give the SageMaker console access to list the available AWS KMS keys\.

`logs`  
Needed to allow SageMaker jobs and endpoints to publish log streams\.