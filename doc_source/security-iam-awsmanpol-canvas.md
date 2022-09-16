# AWS managed policies for Amazon SageMaker Canvas<a name="security-iam-awsmanpol-canvas"></a>

These AWS managed policies add permissions required to use SageMaker Canvas\. The policies are available in your AWS account and are used by execution roles created from the SageMaker console\.

**Topics**
+ [AWS managed policy: AmazonSageMakerCanvasFullAccess](#security-iam-awsmanpol-AmazonSageMakerCanvasFullAccess)
+ [AWS managed policy: AmazonSageMakerCanvasForecastAccess](#security-iam-awsmanpol-AmazonSageMakerCanvasForecastAccess)
+ [Amazon SageMaker updates to Amazon SageMaker Canvas managed policies](#security-iam-awsmanpol-canvas-updates)

## AWS managed policy: AmazonSageMakerCanvasFullAccess<a name="security-iam-awsmanpol-AmazonSageMakerCanvasFullAccess"></a>

This policy grants permissions that allow full access to Amazon SageMaker Canvas through the AWS Management Console and SDK\. The policy also provides select access to related services \(for example, Amazon Simple Storage Service \(Amazon S3\), AWS Identity and Access Management \(IAM\), Amazon Virtual Private Cloud \(Amazon VPC\), Amazon Elastic Container Registry \(Amazon ECR\), Amazon CloudWatch Logs, Amazon Redshift, AWS Secrets Manager, and Amazon Forecast\)\.

**Permissions details**

This AWS managed policy includes the following permissions\.
+ `sagemaker` – Allows principals to create and host SageMaker models\. These resources are limited to those whose name includes "Canvas", "canvas", or "model\-compilation\-"\.
+ `ec2` – Allows principals to create Amazon VPC endpoints\.
+ `ecr` – Allows principals to get information about a container image\.
+ `iam` – Allows principals to pass an IAM role to Amazon SageMaker and Amazon Forecast\.
+ `logs` – Allows principals to publish logs from training jobs and endpoints\.
+ `s3` – Allows principals to add and retrieve objects from Amazon S3 buckets\. These objects are limited to those whose name includes "SageMaker", "Sagemaker", or "sagemaker"\.
+ `secretsmanager` – Allows principals to store customer credentials to connect to a Snowflake database using Secrets Manager\.
+ `redshift-data` – Allows principals to run queries on Amazon Redshift\.
+ `forecast` – Allows principals to use Amazon Forecast\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:DescribeDomain",
                "sagemaker:DescribeUserProfile",
                "sagemaker:ListTags"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateCompilationJob",
                "sagemaker:CreateEndpoint",
                "sagemaker:CreateEndpointConfig",
                "sagemaker:CreateModel",
                "sagemaker:CreateProcessingJob",
                "sagemaker:CreateAutoMLJob",
                "sagemaker:DeleteEndpoint",
                "sagemaker:DescribeCompilationJob",
                "sagemaker:DescribeEndpoint",
                "sagemaker:DescribeEndpointConfig",
                "sagemaker:DescribeModel",
                "sagemaker:DescribeProcessingJob",
                "sagemaker:DescribeAutoMLJob",
                "sagemaker:ListCandidatesForAutoMLJob",
                "sagemaker:AddTags",
                "sagemaker:DeleteApp"
            ],
            "Resource": [
                "arn:aws:sagemaker:*:*:*Canvas*",
                "arn:aws:sagemaker:*:*:*canvas*",
                "arn:aws:sagemaker:*:*:*model-compilation-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateVpcEndpoint",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcs",
                "ec2:DescribeVpcEndpoints",
                "ec2:DescribeVpcEndpointServices"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchGetImage",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetAuthorizationToken"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:GetRole"
            ],
            "Resource": "arn:aws:iam::*:role/*"
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
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:log-group:/aws/sagemaker/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:CreateBucket",
                "s3:GetBucketCors",
                "s3:GetBucketLocation"
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
                "s3:ListBucket",
                "s3:ListAllMyBuckets"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:DescribeSecret",
                "secretsmanager:GetSecretValue",
                "secretsmanager:CreateSecret",
                "secretsmanager:PutResourcePolicy"
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
                "redshift-data:ExecuteStatement",
                "redshift-data:DescribeStatement",
                "redshift-data:CancelStatement",
                "redshift-data:GetStatementResult",
                "redshift-data:ListSchemas",
                "redshift-data:ListTables",
                "redshift-data:DescribeTable"
            ],
            "Resource": "*"
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
                "forecast:CreateExplainabilityExport",
                "forecast:CreateExplainability",
                "forecast:CreateForecastEndpoint",
                "forecast:CreateAutoPredictor",
                "forecast:CreateDatasetImportJob",
                "forecast:CreateDatasetGroup",
                "forecast:CreateDataset",
                "forecast:CreateForecast",
                "forecast:CreateForecastExportJob",
                "forecast:CreatePredictorBacktestExportJob",
                "forecast:CreatePredictor",
                "forecast:DescribeExplainabilityExport",
                "forecast:DescribeExplainability",
                "forecast:DescribeAutoPredictor",
                "forecast:DescribeForecastEndpoint",
                "forecast:DescribeDatasetImportJob",
                "forecast:DescribeDataset",
                "forecast:DescribeForecast",
                "forecast:DescribeForecastExportJob",
                "forecast:DescribePredictorBacktestExportJob",
                "forecast:GetAccuracyMetrics",
                "forecast:InvokeForecastEndpoint",
                "forecast:GetRecentForecastContext",
                "forecast:DescribePredictor",
                "forecast:TagResource"
            ],
            "Resource": [
                "arn:aws:forecast:*:*:*Canvas*"
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
                    "iam:PassedToService": "forecast.amazonaws.com"
                }
            }
        }
    ]
}
```

## AWS managed policy: AmazonSageMakerCanvasForecastAccess<a name="security-iam-awsmanpol-AmazonSageMakerCanvasForecastAccess"></a>

This policy grants permissions commonly needed to use SageMaker Canvas with Amazon Forecast\.

**Permissions details**

This AWS managed policy includes the following permissions\.
+ `s3` – Allows principals to add and retrieve objects from Amazon S3 buckets\. These objects are limited to those whose case\-insensitive name starts with "sagemaker\-"\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::sagemaker-*/Canvas",
                "arn:aws:s3:::sagemaker-*/canvas"
            ]
        }
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::sagemaker-*"
            ]
        }
    ]
}
```

## Amazon SageMaker updates to Amazon SageMaker Canvas managed policies<a name="security-iam-awsmanpol-canvas-updates"></a>

View details about updates to AWS managed policies for SageMaker Canvas since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the SageMaker [Document history page\.](doc-history.md)


| Policy | Version | Change | Date | 
| --- | --- | --- | --- | 
|  [AmazonSageMakerCanvasFullAccess](#security-iam-awsmanpol-AmazonSageMakerCanvasFullAccess)  | 1 |  Initial policy  | September 8, 2022 | 
|  [AmazonSageMakerCanvasForecastAccess](#security-iam-awsmanpol-AmazonSageMakerCanvasForecastAccess)  | 1 |  Initial policy  | August 24, 2022 | 