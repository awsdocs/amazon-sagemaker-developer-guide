# Cross\-Account Offline Store Access<a name="feature-store-security-cross-account"></a>

 Amazon SageMaker Feature Store allows users to create a feature group in one account \(Account A\) and configure it with an offline store using an Amazon S3 bucket in another account \(Account B\)\. This can be set up using the steps in the following section\.

**Topics**
+ [Step 1: Set Up the Offline Store Access Role in Account A](#feature-store-setup-step1)
+ [Step 2: Set up an Offline Store S3 Bucket in Account B](#feature-store-setup-step2)
+ [Step 3: Set up an Offline Store KMS Encryption Key in Account A](#feature-store-setup-step3)
+ [Step 4: Create a Feature Group in Account A](#feature-store-setup-step4)

## Step 1: Set Up the Offline Store Access Role in Account A<a name="feature-store-setup-step1"></a>

First, set up a role for Amazon SageMaker Feature Store to write the data into the offline store\. The simplest way to accomplish this is to create a new role using the `AmazonSageMakerFeatureStoreAccess` policy or to use an existing role that already has the `AmazonSageMakerFeatureStoreAccess` policy attached\. This document refers to this policy as `Account-A-Offline-Feature-Store-Role-ARN`\. 

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
        }
    ]
}
```

The preceding code snippet shows the `AmazonSageMakerFeatureStoreAccess` policy\. The `Resource` section of the policy is scoped down by default to S3 buckets with names that contain `SageMaker`, `Sagemaker`, or `sagemaker`\. This means the offline store S3 bucket being used must follow this naming convention\. If this is not your case, or if you want to further scope down the resource, you can copy and paste the policy to your S3 bucket policy in the console, customize the `Resource` section to be `arn:aws:s3:::your-offline-store-bucket-name`, and then attach to the role\. 

Additionally, this role must have KMS permissions attached\. At a minimum, it requires the `kms:GenerateDataKey` permission to be able to write to the offline store using your customer managed CMK\. See Step 3 to learn about why a customer managed CMK is needed for the cross\-account scenario and how to set it up\. The following example shows an inline policy: 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "kms:GenerateDataKey"
            ],
            "Resource": "arn:aws:kms:*:Account-A-Account-Id:key/*"
        }
    ]
}
```

The `Resource` section of this policy is scoped to any key in Account A\. To further scope this down, after setting up the offline store KMS key in Step 3, return to this policy and replace it with the key ARN\.

## Step 2: Set up an Offline Store S3 Bucket in Account B<a name="feature-store-setup-step2"></a>

Create an S3 bucket in Account B\. If you are using the default `AmazonSageMakerFeatureStoreAccess` policy, the bucket name must include `SageMaker`, `Sagemaker`, or `sagemaker`\. Edit the bucket policy as shown in the following example to allow Account A to read and write objects\.

This document refers to the following example bucket policy as `Account-B-Offline-Feature-Store-Bucket`\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "S3CrossAccountBucketAccess",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl",
                "s3:GetBucketAcl"
            ],
            "Principal": {
                "AWS": [
                    "*Account-A-Offline-Feature-Store-Role-ARN*"
                ],
            },
            "Resource": [
                "arn:aws:s3:::offline-store-bucket-name/*",
                "arn:aws:s3:::offline-store-bucket-name"
            ]
        }
    ]
}
```

In the preceding policy, the principal is `Account-A-Offline-Feature-Store-Role-ARN`, which is the role created in Account A in Step 1 and provided to Amazon SageMaker Feature Store to write to the offline store\. You can provide multiple ARN roles under `Principal`\.

## Step 3: Set up an Offline Store KMS Encryption Key in Account A<a name="feature-store-setup-step3"></a>

Amazon SageMaker Feature Store ensures that server\-side encryption is always enabled for S3 objects in the offline store\. For cross\-account use cases, you must provide a customer managed CMK so that you are in control of who can write to the offline store \(in this case, `Account-A-Offline-Feature-Store-Role-ARN` from Account A\) and who can read from the offline store \(in this case, identities from Account B\)\. 

This document refers to the following example key policy as `Account-A-Offline-Feature-Store-KMS-Key-ARN`\.

```
{
    "Version": "2012-10-17",
    "Id": "key-consolepolicy-3",
    "Statement": [
        {
            "Sid": "Enable IAM User Permissions",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::Account-A-Account-Id:root"
            },
            "Action": "kms:*",
            "Resource": "*"
        },
        {
            "Sid": "Allow access for Key Administrators",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::Account-A-Account-Id:role/Administrator",
                ]
            },
            "Action": [
                "kms:Create*",
                "kms:Describe*",
                "kms:Enable*",
                "kms:List*",
                "kms:Put*",
                "kms:Update*",
                "kms:Revoke*",
                "kms:Disable*",
                "kms:Get*",
                "kms:Delete*",
                "kms:TagResource",
                "kms:UntagResource",
                "kms:ScheduleKeyDeletion",
                "kms:CancelKeyDeletion"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Allow Feature Store to get information about the CMK",
            "Effect": "Allow",
            "Principal": {
                "Service": "sagemaker.amazonaws.com"
            },
            "Action": [
                "kms:Describe*",
                "kms:Get*",
                "kms:List*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Allow use of the key",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "*Account-A-Offline-Feature-Store-Role-ARN*",
                    "*arn:aws:iam::Account-B-Account-Id:root*"
                ]
            },
            "Action": [
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:DescribeKey",
                "kms:CreateGrant",
                "kms:RetireGrant",
                "kms:ReEncryptFrom",
                "kms:ReEncryptTo",
                "kms:GenerateDataKey",
                "kms:ListAliases",
                "kms:ListGrants"
            ],
            "Resource": "*",
        }
    ]
}
```

## Step 4: Create a Feature Group in Account A<a name="feature-store-setup-step4"></a>

Next, create the feature group in Account A, with an offline store S3 bucket in Account B\. To do this, provide the following parameters for `RoleArn`, `OfflineStoreConfig.S3StorageConfig.KmsKeyId` and `OfflineStoreConfig.S3StorageConfig.S3Uri` respectively: 
+ Provide `Account-A-Offline-Feature-Store-Role-ARN` as the `RoleArn`\.
+ Provide `Account-A-Offline-Feature-Store-KMS-Key-ARN` for `OfflineStoreConfig.S3StorageConfig.KmsKeyId`\.
+ Provide `Account-B-Offline-Feature-Store-Bucket` for `OfflineStoreConfig.S3StorageConfig.S3Uri`\.