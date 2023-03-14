# AWS managed policies for Amazon SageMaker geospatial<a name="security-iam-awsmanpol-geospatial"></a>

These AWS managed policies add permissions required to use SageMaker geospatial\. The policies are available in your AWS account and are used by execution roles created from the SageMaker console\.

**Topics**
+ [AWS managed policy: AmazonSageMakerGeospatialFullAccess](#security-iam-awsmanpol-AmazonSageMakerGeospatialFullAccess)
+ [AWS managed policy: AmazonSageMakerGeospatialExecutionRole](#security-iam-awsmanpol-AmazonSageMakerGeospatialExecutionRole)
+ [Amazon SageMaker updates to Amazon SageMaker geospatial managed policies](#security-iam-awsmanpol-geospatial-updates)

## AWS managed policy: AmazonSageMakerGeospatialFullAccess<a name="security-iam-awsmanpol-AmazonSageMakerGeospatialFullAccess"></a>

This policy grants permissions that allow full access to Amazon SageMaker geospatial through the AWS Management Console and SDK\.

**Permissions details**

This AWS managed policy includes the following permissions\.
+ `sagemaker-geospatial` – Allows principals full access to all SageMaker geospatial resources\.
+ `iam` – Allows principals to pass an IAM role to SageMaker geospatial\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sagemaker-geospatial:*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": ["iam:PassRole"],
      "Resource": "arn:aws:iam::*:role/*",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": [
            "sagemaker-geospatial.amazonaws.com"
           ]
        }
      }
    }
  ]
}
```

## AWS managed policy: AmazonSageMakerGeospatialExecutionRole<a name="security-iam-awsmanpol-AmazonSageMakerGeospatialExecutionRole"></a>

This policy grants permissions commonly needed to use SageMaker geospatial\.

**Permissions details**

This AWS managed policy includes the following permissions\.
+ `s3` – Allows principals to add and retrieve objects from Amazon S3 buckets\. These objects are limited to those whose name contains "SageMaker", "Sagemaker", or "sagemaker"\.
+ `sagemaker-geospatial` – Allows principals to access Earth observation jobs through the `GetEarthObservationJob` API\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
          "s3:AbortMultipartUpload",
          "s3:PutObject",
          "s3:GetObject",
          "s3:ListBucketMultipartUploads"
      ],
      "Resource": [
        "arn:aws:s3:::*SageMaker*",
        "arn:aws:s3:::*Sagemaker*",
        "arn:aws:s3:::*sagemaker*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "sagemaker-geospatial:GetEarthObservationJob",
      "Resource": "arn:aws:sagemaker-geospatial:*:*:earth-observation-job/*"
    }
  ]
}
```

## Amazon SageMaker updates to Amazon SageMaker geospatial managed policies<a name="security-iam-awsmanpol-geospatial-updates"></a>

View details about updates to AWS managed policies for SageMaker geospatial since this service began tracking these changes\.


| Policy | Version | Change | Date | 
| --- | --- | --- | --- | 
|  [AmazonSageMakerGeospatialFullAccess](#security-iam-awsmanpol-AmazonSageMakerGeospatialFullAccess) \- New policy  | 1 |  Initial policy  | November 30, 2022 | 
|  [AmazonSageMakerGeospatialExecutionRole](#security-iam-awsmanpol-AmazonSageMakerGeospatialExecutionRole) \- New policy  | 1 |  Initial policy  | November 30, 2022 | 