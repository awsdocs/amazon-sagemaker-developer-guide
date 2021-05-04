# Create a SageMaker Execution Role for a Ground Truth Labeling Job<a name="sms-security-permission-execution-role"></a>

When you configure your labeling job, you need to provide an *execution role*, which is a role that SageMaker has permission to assume to start and run your labeling job\.

This role must give Ground Truth permission to access the following: 
+ Amazon S3 to retrieve your input data and write output data to an Amazon S3 bucket\. You can either grant permission for an IAM role to access an entire bucket by providing the bucket ARN, or you can grant access to the role to access specific resources in a bucket\. For example, the ARN for a bucket may look similar to `arn:aws:s3:::awsexamplebucket1` and the ARN of a resource in an Amazon S3 bucket may look similar to `arn:aws:s3:::awsexamplebucket1/prefix/file-name.png`\. To apply an action to all resources in an Amazon S3 bucket, you can use the wild card: `*`\. For example, `arn:aws:s3:::awsexamplebucket1/prefix/*`\. For more information, see [Amazon Amazon S3 Resources](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-arn-format.html) in the Amazon Simple Storage Service Developer Guide\.
+ CloudWatch to log worker metrics and labeling job statuses\.
+ AWS KMS for data encryption\. \(Optional\)
+ AWS Lambda for processing input and output data when you create a custom workflow\. 

Additionally, if you create a [streaming labeling job](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-streaming-labeling-job.html), this role must have permission to access:
+ Amazon SQS to create an interact with an SQS queue used to [manage labeling requests](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-streaming-labeling-job.html#sms-streaming-how-it-works-sqs)\.
+ Amazon SNS to subscribe to and retrieve messages from your Amazon SNS input topic and to send messages to your Amazon SNS output topic\.

All of these permissions can be granted with the `[AmazonSageMakerGroundTruthExecution](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerGroundTruthExecution)` managed policy *except*:
+ Data and storage volume encryption of your Amazon S3 buckets\. To learn how to configure these permissions, see [Encrypt Output Data and Storage Volume with AWS KMS](sms-security-kms-permissions.md)\.
+ Permission to select and invoke Lambda functions that do not include `GtRecipe`, `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction` in the function name\.
+ Amazon S3 buckets that do not include either `GroundTruth`, `Groundtruth`, `groundtruth`, `SageMaker`, `Sagemaker`, and `sagemaker` in the prefix or bucket name or an [object tag](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-tagging.html) that includes `SageMaker` in the name \(case insensitive\)\.

If you require more granular permissions than the ones provided in `AmazonSageMakerGroundTruthExecution`, use the following policy examples to create an execution role that fits your specific use case\.

**Topics**
+ [Built\-In Task Types \(Non\-streaming\) Execution Role Requirements](#sms-security-permission-execution-role-built-in-tt)
+ [Built\-In Task Types \(Streaming\) Execution Role Requirements](#sms-security-permission-execution-role-built-in-tt-streaming)
+ [Execution Role Requirements for Custom Task Types](#sms-security-permission-execution-role-custom-tt)
+ [Automated Data Labeling Permission Requirements](#sms-security-permission-execution-role-custom-auto-labeling)

## Built\-In Task Types \(Non\-streaming\) Execution Role Requirements<a name="sms-security-permission-execution-role-built-in-tt"></a>

The following policy grants permission to create a labeling job for a [built\-in task type](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html)\. This execution policy does not include permissions for AWS KMS data encryption or decryption\. Replace each red, italicized ARN with your own Amazon S3 ARNs\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "S3ViewBuckets",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::<input-bucket-name>",
                "arn:aws:s3:::<output-bucket-name>"
            ]
        },
        {
            "Sid": "S3GetPutObjects",
            "Effect": "Allow",
            "Action": [
                "s3:AbortMultipartUpload",
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::<input-bucket-name>/*",
                "arn:aws:s3:::<output-bucket-name>/*"
            ]
        },
        {
            "Sid": "CloudWatch",
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

## Built\-In Task Types \(Streaming\) Execution Role Requirements<a name="sms-security-permission-execution-role-built-in-tt-streaming"></a>

If you create a streaming labeling job, you must add a policy similar to the following to the execution role you use to create the labeling job\. To narrow the scope of the policy, replace the `*` in `Resource` with specific AWS resources that you want to grant the IAM role permission to access and use\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:AbortMultipartUpload",
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::<input-bucket-name>/*",
                "arn:aws:s3:::<output-bucket-name>/*"
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
                "s3:GetBucketLocation",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::<input-bucket-name>",
                "arn:aws:s3:::<output-bucket-name>"
            ]
        },
        {
            "Sid": "CloudWatch",
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        },
        {
            "Sid": "StreamingQueue",
            "Effect": "Allow",
            "Action": [
                "sqs:CreateQueue",
                "sqs:DeleteMessage",
                "sqs:GetQueueAttributes",
                "sqs:GetQueueUrl",
                "sqs:ReceiveMessage",
                "sqs:SendMessage",
                "sqs:SendMessageBatch",
                "sqs:SetQueueAttributes"
            ],
            "Resource": "arn:aws:sqs:*:*:*GroundTruth*"
        },
        {
            "Sid": "StreamingTopicSubscribe",
            "Effect": "Allow",
            "Action": "sns:Subscribe",
            "Resource": [
                "arn:aws:sqs:<aws-region>:<aws-account-number>:<input-topic-name>",
                "arn:aws:sqs:<aws-region>:<aws-account-number>:<output-topic-name>"
            ],
            "Condition": {
                "StringEquals": {
                    "sns:Protocol": "sqs"
                },
                "StringLike": {
                    "sns:Endpoint": "arn:aws:sqs:<aws-region>:<aws-account-number>:*GroundTruth*"
                }
            }
        },
        {
            "Sid": "StreamingTopic",
            "Effect": "Allow",
            "Action": [
                "sns:Publish"
            ],
            "Resource": [
                "arn:aws:sqs:<aws-region>:<aws-account-number>:<input-topic-name>",
                "arn:aws:sqs:<aws-region>:<aws-account-number>:<output-topic-name>"
            ]
        },
        {
            "Sid": "StreamingTopicUnsubscribe",
            "Effect": "Allow",
            "Action": [
                "sns:Unsubscribe"
            ],
            "Resource": [
                "arn:aws:sqs:<aws-region>:<aws-account-number>:<input-topic-name>",
                "arn:aws:sqs:<aws-region>:<aws-account-number>:<output-topic-name>"
            ]
        }
    ]
}
```

## Execution Role Requirements for Custom Task Types<a name="sms-security-permission-execution-role-custom-tt"></a>

If you want to create a [custom labeling workflow](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-custom-templates.html), add the following statement to an execution role policy like the ones found in [](#sms-security-permission-execution-role-built-in-tt) or [Built\-In Task Types \(Streaming\) Execution Role Requirements](#sms-security-permission-execution-role-built-in-tt-streaming)\.

This policy gives the execution role permission to `Invoke` your pre\-annotation and post\-annotation Lambda functions\.

```
{
    "Sid": "LambdaFunctions",
    "Effect": "Allow",
    "Action": [
        "lambda:InvokeFunction"
    ],
    "Resource": [
        "arn:aws:lambda:<region>:<account-id>:function:<pre-annotation-lambda-name>",
        "arn:aws:lambda:<region>:<account-id>:function:<post-annotation-lambda-name>"
    ]
}
```

## Automated Data Labeling Permission Requirements<a name="sms-security-permission-execution-role-custom-auto-labeling"></a>

If you want to create a labeling job with [automated data labeling](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-automated-labeling.html) enabled, you must 1\) add one policy to the IAM policy attached to the execution role and 2\) update the trust policy of the execution role\. 

The following statement allows the IAM execution role to be passed to SageMaker so that it can be used to run the training and inference jobs used for active learning and automated data labeling respectively\. Add this statement to an execution role policy like the ones found in [](#sms-security-permission-execution-role-built-in-tt) or [Built\-In Task Types \(Streaming\) Execution Role Requirements](#sms-security-permission-execution-role-built-in-tt-streaming)\. Replace `arn:aws:iam::<account-number>:role/<role-name>` with the execution role ARN\. You can find your IAM role ARN in the IAM console under **Roles**\. 

```
{
    "Effect": "Allow",
    "Action": [
        "iam:PassRole"
    ],
    "Resource": "arn:aws:iam::<account-number>:role/<execution-role-name>",
    "Condition": {
        "StringEquals": {
            "iam:PassedToService": [
                "sagemaker.amazonaws.com"
            ]
        }
    }
}
```

The following statement allows SageMaker to assume the execution role to create and manage the SageMaker training and inference jobs\. This policy must be added to the trust relationship of the execution role\. To learn how to add or modify an IAM role trust policy, see [Modifying a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Principal": {"Service": "sagemaker.amazonaws.com" },
        "Action": "sts:AssumeRole"
    }
}
```

