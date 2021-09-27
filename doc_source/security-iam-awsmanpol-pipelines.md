# AWS Managed Policies for SageMaker Pipelines<a name="security-iam-awsmanpol-pipelines"></a>

These AWS managed policies add permissions required to use SageMaker Pipelines\. The policies are available in your AWS account and are used by execution roles created from the SageMaker console\.

**Topics**
+ [AWS managed policy: AmazonSageMakerPipelinesIntegrations](#security-iam-awsmanpol-AmazonSageMakerPipelinesIntegrations)
+ [Amazon SageMaker updates to SageMaker Pipelines managed policies](#security-iam-awsmanpol-pipelines-updates)

## AWS managed policy: AmazonSageMakerPipelinesIntegrations<a name="security-iam-awsmanpol-AmazonSageMakerPipelinesIntegrations"></a>

This AWS managed policy grants permissions commonly needed to use Callback steps and Lambda steps in SageMaker Pipelines\. The policy is added to the `AmazonSageMaker-ExecutionRole` that is created when you onboard to Amazon SageMaker Studio\. The policy can be attached to any role used for authoring or executing a pipeline\.

This policy grants appropriate AWS Lambda, Amazon Simple Queue Service \(Amazon SQS\), and IAM permissions needed when building pipelines that invoke Lambda functions or include callback steps, which can be used for manual approval steps or running custom workloads\.

The Amazon SQS permissions allow you to create the Amazon SQS queue needed for receiving callback messages, and also to send messages to that queue\.

The Lambda permissions allow you to create, update, and delete the Lambda functions used in the pipeline steps, and also to invoke those Lambda functions\.

**Permissions details**

This policy includes the following permissions\.
+ `lambda` – Create, delete, and invoke Lambda functions; update Lambda function code\. These permissions are limited to functions whose name includes "sagemaker"\.
+ `sqs` – Create an Amazon SQS queue; send an Amazon SQS message\. These permissions are limited to queues whose name includes "sagemaker"\.
+ `iam` – Pass an IAM role to the AWS Lambda service\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "lambda:CreateFunction",
                "lambda:DeleteFunction",
                "lambda:InvokeFunction",
                "lambda:UpdateFunctionCode"
            ],
            "Resource": [
                "arn:aws:lambda:*:*:function:*sagemaker*",
                "arn:aws:lambda:*:*:function:*sageMaker*",
                "arn:aws:lambda:*:*:function:*SageMaker*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "sqs:CreateQueue",
                "sqs:SendMessage"
            ],
            "Resource": [
                "arn:aws:sqs:*:*:*sagemaker*",
                "arn:aws:sqs:*:*:*sageMaker*",
                "arn:aws:sqs:*:*:*SageMaker*"
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
                        "lambda.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

## Amazon SageMaker updates to SageMaker Pipelines managed policies<a name="security-iam-awsmanpol-pipelines-updates"></a>

View details about updates to AWS managed policies for Amazon SageMaker since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the SageMaker [Document history page\.](doc-history.md)


| Change | Description | Date | 
| --- | --- | --- | 
|  SageMaker started tracking changes  |  SageMaker started tracking changes for its SageMaker Pipelines AWS managed policies\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/security-iam-awsmanpol-pipelines.html)  | July 30, 2021 | 