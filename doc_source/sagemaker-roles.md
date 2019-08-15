# Amazon SageMaker Roles<a name="sagemaker-roles"></a>

As a managed service, Amazon SageMaker performs operations on your behalf on the AWS hardware that is managed by Amazon SageMaker\. Amazon SageMaker can perform only operations that the user permits\.

An Amazon SageMaker user can grant these permissions with an IAM role \(referred to as an execution role\)\. The user passes the role when making these API calls: [CreateNotebookInstance](API_CreateNotebookInstance.md), [CreateHyperParameterTuningJob](API_CreateHyperParameterTuningJob.md), [CreateTrainingJob](API_CreateTrainingJob.md), and [CreateModel](API_CreateModel.md)\.

You attach the following trust policy to the IAM role which grants Amazon SageMaker principal permissions to assume the role, and is the same for all of the execution roles: 

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
Instead of managing permissions by crafting a permission policy, you can use the AWS\-managed `AmazonSageMakerFullAccess` permission policy\. The permissions in this policy are fairly well scoped to the actions you need to perform\. For more information see [Using the AWS Managed Permission Policy \(AmazonSageMakerFullAccess\) for an Execution Role](#sagemaker-roles-amazonsagemakerfullaccess-policy)\. If you prefer to create custom policies and manage permissions, see the following topics\. 

For more information about IAM roles, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

**Topics**
+ [CreateNotebookInstance API: Execution Role Permissions](#sagemaker-roles-createnotebookinstance-perms)
+ [CreateHyperParameterTuningJob API: Execution Role Permissions](#sagemaker-roles-createhyperparametertiningjob-perms)
+ [CreateTrainingJob API: Execution Role Permissions](#sagemaker-roles-createtrainingjob-perms)
+ [CreateModel API: Execution Role Permissions](#sagemaker-roles-createmodel-perms)
+ [Using the AWS Managed Permission Policy \(AmazonSageMakerFullAccess\) for an Execution Role](#sagemaker-roles-amazonsagemakerfullaccess-policy)

## CreateNotebookInstance API: Execution Role Permissions<a name="sagemaker-roles-createnotebookinstance-perms"></a>

The permissions that you grant to the execution role for calling the `CreateNotebookInstance` API depend on what you plan to do with the notebook instance\. If you plan to use it to invoke Amazon SageMaker APIs and pass the same role when calling the `CreateTrainingJob` and `CreateModel` APIs, attach the following permissions policy to the role:

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
                "ec2:DescribeRouteTables"
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

To tighten the permissions, limit them to specific Amazon S3 and Amazon ECR resources, by replacing `"Resource": "*"`, as follows:

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
+ Scope S3 permissions to objects that you specify in the `PrimaryContainer.ModelDataUrl` in a [CreateModel](API_CreateModel.md) request\.
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

## Using the AWS Managed Permission Policy \(AmazonSageMakerFullAccess\) for an Execution Role<a name="sagemaker-roles-amazonsagemakerfullaccess-policy"></a>

You can create an execution role one of two ways:
+ In the Amazon SageMaker console when you create a notebook instance, training job, or model\.
+ In the AWS Identity and Access Management \(IAM\) console\. You then specify the role as you follow the notebook instance, training job, and model creation workflows in the Amazon SageMaker console\.

Regardless of how you create an execution role, you can attach the AWS\-managed permission policy \(AmazonSageMakerFullAccess\) to the role\.

When attaching the AmazonSageMakerFullAccess policy to a role, you must do one of the following to allow Amazon SageMaker to access your S3 bucket:
+ Include the string `"SageMaker"` or `"sagemaker"` in the name of the bucket where you store training data, or the model artifacts resulting from model training, or both\.
+ Include the string `"SageMaker"` or `"sagemaker"` in the object name of the training data object\(s\)\.
+ Tag the S3 object with "SageMaker=true"\. The key and value are case sensitive\. For more information, see [Object Tagging](http://docs.aws.amazon.com/AmazonS3/latest/dev/object-tagging.html) in the Amazon Simple Storage Service Developer Guide\.
+ Add a bucket policy that allows access for the execution role\. For more information, see [Using Bucket Policies and User Policies](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the Amazon Simple Storage Service Developer Guide\. 

You can attach additional policies that specify the resources for which you want to grant permissions for the `s3:GetObject`, `s3:PutObject`, and `s3:ListBucket` actions\. In the IAM console, you can attach a customer managed policy or an inline policy to your execution role\(s\)\. Alternatively, when you create a role in the Amazon SageMaker console, you can attach a customer managed policy that specifies the S3 buckets\. This resulting execution role has the prefix `"AmazonSageMaker-ExecutionRole-"`\.
