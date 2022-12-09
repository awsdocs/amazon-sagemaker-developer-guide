# AWS Managed Policies for SageMaker Model Governance<a name="security-iam-awsmanpol-governance"></a>

This AWS managed policy adds permissions required to use SageMaker Model Governance\. The policy is available in your AWS account and is used by execution roles created from the SageMaker console\.

**Topics**
+ [AWS managed policy: AmazonSageMakerModelGovernanceUseAccess](#security-iam-awsmanpol-governance-AmazonSageMakerModelGovernanceUseAccess)
+ [Amazon SageMaker updates to SageMaker Model Governance managed policies](#security-iam-awsmanpol-governance-updates)

## AWS managed policy: AmazonSageMakerModelGovernanceUseAccess<a name="security-iam-awsmanpol-governance-AmazonSageMakerModelGovernanceUseAccess"></a>

This AWS managed policy grants permissions needed to use all Amazon SageMaker Governance features\. The policy is available in your AWS account\.

This policy includes the following permissions\.
+ `s3` – Retrieve objects from Amazon S3 buckets\. Retrievable objects are limited to those whose case\-insensitive name contains the string `"sagemaker"`\.
+ `kms` – List the AWS KMS keys to use for content encryption\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:ListMonitoringAlerts",
                "sagemaker:ListMonitoringExecutions",
                "sagemaker:UpdateMonitoringAlert",
                "sagemaker:StartMonitoringSchedule",
                "sagemaker:StopMonitoringSchedule",
                "sagemaker:ListMonitoringAlertHistory",
                "sagemaker:CreateModelCard",
                "sagemaker:DescribeModelCard",
                "sagemaker:UpdateModelCard",
                "sagemaker:DeleteModelCard",
                "sagemaker:ListModelCards",
                "sagemaker:ListModelCardVersions",
                "sagemaker:CreateModelCardExportJob",
                "sagemaker:DescribeModelCardExportJob",
                "sagemaker:ListModelCardExportJobs"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:ListTrainingJobs",
                "sagemaker:DescribeTrainingJob",
                "sagemaker:ListModels",
                "sagemaker:DescribeModel",
                "sagemaker:Search",     
                "sagemaker:AddTags",
                "sagemaker:DeleteTags",
                "sagemaker:ListTags"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:ListAliases"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:CreateBucket",
                "s3:GetBucketLocation",
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
        }
    ]
}
```

## Amazon SageMaker updates to SageMaker Model Governance managed policies<a name="security-iam-awsmanpol-governance-updates"></a>

View details about updates to AWS managed policies for SageMaker Model Governance since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the SageMaker [Document history page\.](doc-history.md)


| Policy | Version | Change | Date | 
| --- | --- | --- | --- | 
| [AmazonSageMakerModelGovernanceUseAccess](#security-iam-awsmanpol-governance-AmazonSageMakerModelGovernanceUseAccess) \- New policy | 1 |  Initial policy  | November 30, 2022 | 