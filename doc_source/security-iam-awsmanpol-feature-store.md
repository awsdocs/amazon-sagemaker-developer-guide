# AWS managed policies for Amazon SageMaker Feature Store<a name="security-iam-awsmanpol-feature-store"></a>

These AWS managed policies add permissions required to use Feature Store\. The policies are available in your AWS account and are used by execution roles created from the SageMaker console\.

**Topics**
+ [AWS managed policy: AmazonSageMakerFeatureStoreAccess](#security-iam-awsmanpol-AmazonSageMakerFeatureStoreAccess)
+ [Amazon SageMaker updates to Amazon SageMaker Feature Store managed policies](#security-iam-awsmanpol-feature-store-updates)

## AWS managed policy: AmazonSageMakerFeatureStoreAccess<a name="security-iam-awsmanpol-AmazonSageMakerFeatureStoreAccess"></a>

This policy grants permissions required to enable the offline store for an Amazon SageMaker Feature Store feature group\.

**Permissions details**

This AWS managed policy includes the following permissions\.
+ `s3` – Allows principals to write data into an offline store Amazon S3 bucket\. These buckets are limited to those whose name includes "SageMaker", "Sagemaker", or "sagemaker"\.
+ `s3` – Allows principals to read existing manifest files maintained in the `metadata` folder of an offline store S3 bucket\.
+ `glue` – Allows principals to read and update AWS Glue tables\. These permissions are limited to tables in the `sagemaker_featurestore` folder\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
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
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::*SageMaker*/metadata/*",
                "arn:aws:s3:::*Sagemaker*/metadata/*",
                "arn:aws:s3:::*sagemaker*/metadata/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:GetTable",
                "glue:UpdateTable"
            ],
            "Resource": [
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/sagemaker_featurestore",
                "arn:aws:glue:*:*:table/sagemaker_featurestore/*"
            ]
        }
    ]
}
```

## Amazon SageMaker updates to Amazon SageMaker Feature Store managed policies<a name="security-iam-awsmanpol-feature-store-updates"></a>

View details about updates to AWS managed policies for Feature Store since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the SageMaker [Document history page\.](doc-history.md)


| Policy | Version | Change | Date | 
| --- | --- | --- | --- | 
|  [AmazonSageMakerFeatureStoreAccess](#security-iam-awsmanpol-AmazonSageMakerFeatureStoreAccess) \- Update to an existing policy  | 3 |  Add `s3:GetObject`, `glue:GetTable`, and `glue:UpdateTable` permissions\.  | December 5, 2022 | 
| AmazonSageMakerFeatureStoreAccess \- Update to an existing policy | 2 |  Add `s3:PutObjectAcl` permission\.  | February 23, 2021 | 
| AmazonSageMakerFeatureStoreAccess \- New policy | 1 |  Initial policy  | December 1, 2020 | 