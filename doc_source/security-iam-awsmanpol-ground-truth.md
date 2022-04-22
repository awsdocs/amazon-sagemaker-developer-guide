# AWS Managed Policies for Amazon SageMaker Ground Truth<a name="security-iam-awsmanpol-ground-truth"></a>

These AWS managed policies add permissions required to use SageMaker Ground Truth\. The policies are available in your AWS account and are used by execution roles created from the SageMaker console\.

**Topics**
+ [AWS managed policy: AmazonSageMakerGroundTruthExecution](#security-iam-awsmanpol-gt-AmazonSageMakerGroundTruthExecution)
+ [Amazon SageMaker updates to SageMaker Ground Truth managed policies](#security-iam-awsmanpol-groundtruth-updates)

## AWS managed policy: AmazonSageMakerGroundTruthExecution<a name="security-iam-awsmanpol-gt-AmazonSageMakerGroundTruthExecution"></a>

This AWS managed policy grants permissions commonly needed to use SageMaker Ground Truth\.

**Permissions details**

This policy includes the following permissions\.
+ `lambda` – Allows principals to invoke Lambda functions whose name includes "sagemaker" \(case\-insensitive\), "GtRecipe", or "LabelingFunction"\.
+ `s3` – Allows principals to add and retrieve objects from Amazon S3 buckets\. These objects are limited to those whose case\-insensitive name contains "groundtruth" or "sagemaker", or are tagged with "SageMaker"\.
+ `cloudwatch` – Allows principals to post CloudWatch metrics\.
+ `logs` – Allows principals to create and access log streams, and post log events\.
+ `sqs` – Allows principals to create Amazon SQS queues, and send and receive Amazon SQS messages\. These permissions are limited to queues whose name includes "GroundTruth"\.
+ `sns` – Allows principals to subscribe to and publish messages to Amazon SNS topics whose case\-insensitive name contains "groundtruth" or "sagemaker"\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CustomLabelingJobs",
            "Effect": "Allow",
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": [
                "arn:aws:lambda:*:*:function:*GtRecipe*",
                "arn:aws:lambda:*:*:function:*LabelingFunction*",
                "arn:aws:lambda:*:*:function:*SageMaker*",
                "arn:aws:lambda:*:*:function:*sagemaker*",
                "arn:aws:lambda:*:*:function:*Sagemaker*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:AbortMultipartUpload",
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::*GroundTruth*",
                "arn:aws:s3:::*Groundtruth*",
                "arn:aws:s3:::*groundtruth*",
                "arn:aws:s3:::*SageMaker*",
                "arn:aws:s3:::*Sagemaker*",
                "arn:aws:s3:::*sagemaker*"
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
            "Resource": "*"
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
                "sqs:SetQueueAttributes"
            ],
            "Resource": "arn:aws:sqs:*:*:*GroundTruth*"
        },
        {
            "Sid": "StreamingTopicSubscribe",
            "Effect": "Allow",
            "Action": "sns:Subscribe",
            "Resource": [
                "arn:aws:sns:*:*:*GroundTruth*",
                "arn:aws:sns:*:*:*Groundtruth*",
                "arn:aws:sns:*:*:*groundTruth*",
                "arn:aws:sns:*:*:*groundtruth*",
                "arn:aws:sns:*:*:*SageMaker*",
                "arn:aws:sns:*:*:*Sagemaker*",
                "arn:aws:sns:*:*:*sageMaker*",
                "arn:aws:sns:*:*:*sagemaker*"
            ],
            "Condition": {
                "StringEquals": {
                    "sns:Protocol": "sqs"
                },
                "StringLike": {
                    "sns:Endpoint": "arn:aws:sqs:*:*:*GroundTruth*"
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
                "arn:aws:sns:*:*:*GroundTruth*",
                "arn:aws:sns:*:*:*Groundtruth*",
                "arn:aws:sns:*:*:*groundTruth*",
                "arn:aws:sns:*:*:*groundtruth*",
                "arn:aws:sns:*:*:*SageMaker*",
                "arn:aws:sns:*:*:*Sagemaker*",
                "arn:aws:sns:*:*:*sageMaker*",
                "arn:aws:sns:*:*:*sagemaker*"
            ]
        },
        {
            "Sid": "StreamingTopicUnsubscribe",
            "Effect": "Allow",
            "Action": [
                "sns:Unsubscribe"
            ],
            "Resource": "*"
        }
    ]
}
```

## Amazon SageMaker updates to SageMaker Ground Truth managed policies<a name="security-iam-awsmanpol-groundtruth-updates"></a>

View details about updates to AWS managed policies for Amazon SageMaker Ground Truth since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the SageMaker [Document history page\.](doc-history.md)


| Policy | Version | Change | Date | 
| --- | --- | --- | --- | 
| AmazonSageMakerGroundTruthExecution | 2 |  Remove `sqs:SendMessageBatch` permission\.  | April 15, 2022 | 
| AmazonSageMakerGroundTruthExecution | 1 |  Initial policy  | July 20, 2020 | 