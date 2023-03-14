# SageMaker geospatial capabilities roles<a name="sagemaker-geospatial-roles"></a>

As a managed service, Amazon SageMaker geospatial capabilities perform operations on your behalf on the AWS hardware that is managed by SageMaker\. It can perform only operations that the user permits\.

A user can grant these permissions with an IAM role \(referred to as an execution role\)\. 

To create and use a locally available execution role, you can use the following procedures\.

## Create an execution role<a name="sagemaker-geospatial-roles-create-execution-role"></a>

To work with SageMaker geospatial capabilities you need to setup a user role and an execution role\. A user role is an AWS identity with permission policies that determine what the user can and can not do within AWS\. An execution role is an IAM role that grants the service permission to access your AWS resources\. An execution role consists of permissions and trust policy\. The trust policy specifies which principals have the permission to assume the role\.

Use the following procedure to create an execution role with the IAM managed policy, `AmazonSageMakerGeospatialFullAccess`, attached\. If your use case requires more granular permissions, use other sections of this guide to create an execution role that meets your business needs\.

**Important**  
The IAM managed policy, `AmazonSageMakerGeospatialFullAccess`, used in the following procedure only grants the execution role permission to perform certain Amazon S3 actions on buckets or objects with `SageMaker`, `Sagemaker`, `sagemaker`, or `aws-glue` in the name\. To learn how to add an additional policy to an execution role to grant it access to other Amazon S3 buckets and objects, see [Add Additional Amazon S3 Permissions to a SageMaker Execution Role](sagemaker-roles.md#sagemaker-roles-get-execution-role-s3)\.

**To create a new role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Select **Roles** and then select **Create role**\.

1. Select **SageMaker**\.

1. Select **Next: Permissions**\.

1. The IAM managed policy, `AmazonSageMakerGeospatialFullAccess` is automatically attached to this role\. To see the permissions included in this policy, select the sideways arrow next to the policy name\. Select **Next: Tags**\.

1. \(Optional\) Add tags and select **Next: Review**\.

1. Give the role a name in the text field under **Role name** and select **Create role**\.

1. In the **Roles** section of the IAM console, select the role you just created\. If needed, use the text box to search for the role using the role name you entered in step 7\.

1. On the role summary page, make note of the ARN\.

## Passing Roles<a name="sagemaker-geospatial-roles-pass-role"></a>

Actions like passing a role between services are a common function within SageMaker\. You can find more details on [Actions, Resources, and Condition Keys for SageMaker](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonsagemaker.html#amazonsagemaker-actions-as-permissions) in the *IAM User Guide*\.

You pass the role \(`iam:PassRole`\) when making these API calls: [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_StartEarthObservationJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_StartEarthObservationJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_StartVectorEnrichmentJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_StartVectorEnrichmentJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_ExportEarthObservationJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_ExportEarthObservationJob.html), [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_ExportVectorEnrichmentJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_ExportVectorEnrichmentJob.html)\.

You attach the following trust policy to the IAM role, which grants SageMaker principal permissions to assume the role, and is the same for all of the execution roles: 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "sagemaker-geospatial.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

The permissions that you need to grant to the role may vary depending on the API that you call\. The following sections explain these permissions\.

**Note**  
Instead of managing permissions by crafting a permission policy, you can use the AWS managed `AmazonSageMakerFullAccess` permission policy\. The permissions in this policy are fairly broad, to allow for any actions you might want to perform in SageMaker\. For a listing of the policy including information about the reasons for adding many of the permissions, see [AWS managed policy: AmazonSageMakerFullAccess](security-iam-awsmanpol.md#security-iam-awsmanpol-AmazonSageMakerFullAccess)\. If you prefer to create custom policies and manage permissions to scope the permissions only to the actions you need to perform with the execution role, see the following topics\.

**Important**  
If you're running into issues, see [Troubleshooting Amazon SageMaker Identity and Access](security_iam_troubleshoot.md)\.

For more information about IAM roles, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

**Topics**
+ [Create an execution role](#sagemaker-geospatial-roles-create-execution-role)
+ [Passing Roles](#sagemaker-geospatial-roles-pass-role)
+ [`StartEarthObservationJob` API: Execution Role Permissions](#sagemaker-roles-start-eoj-perms)
+ [`StartVectorEnrichmentJob` API: Execution Role Permissions](#sagemaker-roles-start-vej-perms)
+ [`ExportEarthObservationJob` API: Execution Role Permissions](#sagemaker-roles-export-eoj-perms)
+ [`ExportVectorEnrichmentJob` API: Execution Role Permissions](#sagemaker-roles-export-vej-perms)

## `StartEarthObservationJob` API: Execution Role Permissions<a name="sagemaker-roles-start-eoj-perms"></a>

For an execution role that you can pass in a `StartEarthObservationJob` API request, you can attach the following minimum permission policy to the role:

```
{
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
                    "Resource":  "arn:aws:sagemaker-geospatial:*:*:earth-observation-job/*"
                  }
                ]
              }
    
}
```

If your input Amazon S3 bucket is encrypted using server\-side encryption with an AWS KMS managed key \(SSE\-KMS\), see [Using Amazon S3 Bucket Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html) for more information\.

## `StartVectorEnrichmentJob` API: Execution Role Permissions<a name="sagemaker-roles-start-vej-perms"></a>

For an execution role that you can pass in a `StartVectorEnrichmentJob` API request, you can attach the following minimum permission policy to the role:

```
{
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
                    "Action": "sagemaker-geospatial:GetVectorEnrichmentJob",
                    "Resource":  "arn:aws:sagemaker-geospatial:*:*:vector-enrichment-job/*"
                  }
                ]
              }
    
}
```

If your input Amazon S3 bucket is encrypted using server\-side encryption with an AWS KMS managed key \(SSE\-KMS\), see [Using Amazon S3 Bucket Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html) for more information\.

## `ExportEarthObservationJob` API: Execution Role Permissions<a name="sagemaker-roles-export-eoj-perms"></a>

For an execution role that you can pass in a `ExportEarthObservationJob` API request, you can attach the following minimum permission policy to the role:

```
{
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
                    "Resource":  "arn:aws:sagemaker-geospatial:*:*:earth-observation-job/*"
                  }
                ]
              }
    
}
```

If your input Amazon S3 bucket is encrypted using server\-side encryption with an AWS KMS managed key \(SSE\-KMS\), see [Using Amazon S3 Bucket Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html) for more information\.

## `ExportVectorEnrichmentJob` API: Execution Role Permissions<a name="sagemaker-roles-export-vej-perms"></a>

For an execution role that you can pass in a `ExportVectorEnrichmentJob` API request, you can attach the following minimum permission policy to the role:

```
{
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
                    "Action": "sagemaker-geospatial:GetVectorEnrichmentJob",
                    "Resource":  "arn:aws:sagemaker-geospatial:*:*:vector-enrichment-job/*"
                  }
                ]
              }
    
}
```

If your input Amazon S3 bucket is encrypted using server\-side encryption with an AWS KMS managed key \(SSE\-KMS\), see [Using Amazon S3 Bucket Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html) for more information\.