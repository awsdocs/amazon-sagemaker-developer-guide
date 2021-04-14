# Assign IAM Permissions to Use Ground Truth<a name="sms-security-permission"></a>

Use the topics on this page to learn how to use AWS Identity and Access Management \(IAM\) managed and custom policies to manage access to Ground Truth and associated resources\. 

You can use the sections on this page to learn the following: 
+ How to create IAM policies that grant an IAM user or role permission to create a labeling job\. Administrators can use IAM policies to restrict access to Amazon SageMaker and other AWS services that are specific to Ground Truth\.
+ How to create an *execution role* for labeling jobs\. An execution role is the role that you specify when you create a labeling job\. The role is used to execute your labeling job\.

The following is an overview of the topics you'll find on this page: 
+ If you are getting started using Ground Truth, or you do not require granular permissions for your use case, see [Grant General Permissions To Get Started Using Ground Truth](#sms-security-permissions-get-started)\.
+ Use the policy in [Permissions Required to Use the Amazon SageMaker Ground Truth Console and API](#sms-security-permission-console-access) to grant access to the Ground Truth area of the SageMaker console\. This policy includes permissions to create and modify private work teams\. To learn more about these permissions, see [Grant Permissions for Private Workforce Creation](#sms-security-permission-workforce-creation)\.
+ When you create a labeling job, you must provide an execution role\. Use [Create an Execution Role to Start a Labeling Job](#sms-security-permission-execution-role) to learn about the permissions required for this role\.

**Topics**
+ [Grant General Permissions To Get Started Using Ground Truth](#sms-security-permissions-get-started)
+ [Permissions Required to Use the Amazon SageMaker Ground Truth Console and API](#sms-security-permission-console-access)
+ [Create an Execution Role to Start a Labeling Job](#sms-security-permission-execution-role)
+ [Encrypt Output Data and Storage Volume with AWS KMS](#sms-security-kms-permissions)

## Grant General Permissions To Get Started Using Ground Truth<a name="sms-security-permissions-get-started"></a>

If you are getting started using Ground Truth and you do not require granular permissions for your use case, you can attach the managed policy `[AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess)` to an IAM user or role to give that entity permission to create a labeling job\. This is a broad policy that grants an IAM entity permission to use SageMaker features, as well as features of related AWS services through the console and API\. This policy gives the IAM entity permission to create a labeling job and to create and manage workforces using Amazon Cognito\. To learn more, see `[AmazonSageMakerFullAccess Policy](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-amazonsagemakerfullaccess-policy)`\.

To create an *execution role*, you can attach the policy [AmazonSageMakerGroundTruthExecution](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerGroundTruthExecution) to an IAM role\. An execution role is the role that you specify when you create a labeling job and it is used to execute your labeling job\. AmazonSageMakerGroundTruthExecution grants all required permissions *except* for data and storage volume encryption if your S3 buckets, and Lambda functions that do not meet the conditions specified in the policy\. You should be aware of the following Amazon S3 and AWS Lambda permissions included in this policy:
+ **AWS Lambda permissions**: When you create a [custom labeling workflow](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-custom-templates.html), this execution role is restricted to invoking AWS Lambda functions with one of the following strings as part of the function name: `GtRecipe`, `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\. This applies to both your pre\-annotation and post\-annotation Lambda functions\. If you choose to use names without those strings, you must explicitly provide `lambda:InvokeFunction` permission to the execution role used to create the labeling job\.
+ **Amazon S3 permissions**: This policy grants an execution role permission to access Amazon S3 buckets with the following strings in the name: `GroundTruth`, `Groundtruth`, `groundtruth`, `SageMaker`, `Sagemaker`, and `sagemaker`\. Make sure your input and output bucket names include these strings, or add additional permissions to your execution role to [grant it permission to access your S3 buckets](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_examples_s3_rw-bucket.html)\. You must give this role permission to perform the following actions on your S3 buckets: `AbortMultipartUpload`, `GetObject`, and `PutObject`\.

## Permissions Required to Use the Amazon SageMaker Ground Truth Console and API<a name="sms-security-permission-console-access"></a>

To use the Ground Truth area of the SageMaker console, you need to grant permission to an IAM entity to access SageMaker and other AWS services used by Ground Truth\. For example, you must add Amazon S3 actions to grant access to the Amazon S3 buckets that contain input and output data\. Permissions to access other AWS services depends on your use\-case: 
+ AWS Marketplace permissions are required to use a vendor workforce\.
+ Amazon Cognito permission are required for private work team setup\.
+ AWS KMS permissions are required to view available AWS KMS keys that can be used for output data encryption\.
+ IAM permissions are required to either list pre\-existing execution roles, or to create a new one\. Additionally, you must use add a `PassRole` permission to allow SageMaker to use the execution role chosen to execute the labeling job\.

The following sections list policies you may want to grant to an IAM role to use one or more functions of Ground Truth\. 

**Topics**
+ [Grant Permission to Use All Ground Truth Console Features](#sms-security-permissions-console-all)
+ [Create a Custom Labeling Workflow in the Console](#sms-security-permissions-custom-workflow)
+ [Grant Permissions for Private Workforce Creation](#sms-security-permission-workforce-creation)
+ [Grant Permissions to Subscribe to a Vendor Workforce](#sms-security-permissions-workforce-creation-vendor)

### Grant Permission to Use All Ground Truth Console Features<a name="sms-security-permissions-console-all"></a>

To grant permission to an IAM user or role to use the Ground Truth area of the SageMaker console to create a labeling job, attach the following policy to the user or role\. The following policy will give an IAM role permission to create a labeling job using a [built\-in task type](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html) task type\. See the following section for the policy you must include to create a custom labeling workflow\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "SageMakerApis",
            "Effect": "Allow",
            "Action": [
                "sagemaker:*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "KmsKeysForCreateForms",
            "Effect": "Allow",
            "Action": [
                "kms:DescribeKey",
                "kms:ListAliases"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AccessAwsMarketplaceSubscriptions",
            "Effect": "Allow",
            "Action": [
                "aws-marketplace:ViewSubscriptions",
                "aws-marketplace:DescribeListings"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:CreateSecret",
                "secretsmanager:DescribeSecret",
                "secretsmanager:ListSecrets"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ListAndCreateExecutionRoles",
            "Effect": "Allow",
            "Action": [
                "iam:ListRoles",
                "iam:CreateRole",
                "iam:CreatePolicy",
                "iam:AttachRolePolicy"
            ],
            "Resource": "*"
        },
        {
            "Sid": "PassRoleForExecutionRoles",
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
            "Sid": "GroundTruthConsole",
            "Effect": "Allow",
            "Action": [
                "groundtruthlabeling:*",
                "lambda:InvokeFunction",
                "lambda:ListFunctions",
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "s3:GetBucketCors",
                "s3:PutBucketCors",
                "s3:SelectObjectContent",
                "s3:ListAllMyBuckets",
                "cognito-idp:AdminAddUserToGroup",
                "cognito-idp:AdminCreateUser",
                "cognito-idp:AdminDeleteUser",
                "cognito-idp:AdminDisableUser",
                "cognito-idp:AdminEnableUser",
                "cognito-idp:AdminRemoveUserFromGroup",
                "cognito-idp:CreateGroup",
                "cognito-idp:CreateUserPool",
                "cognito-idp:CreateUserPoolClient",
                "cognito-idp:CreateUserPoolDomain",
                "cognito-idp:DescribeUserPool",
                "cognito-idp:DescribeUserPoolClient",
                "cognito-idp:ListGroups",
                "cognito-idp:ListIdentityProviders",
                "cognito-idp:ListUsers",
                "cognito-idp:ListUsersInGroup",
                "cognito-idp:ListUserPoolClients",
                "cognito-idp:ListUserPools",
                "cognito-idp:UpdateUserPool",
                "cognito-idp:UpdateUserPoolClient"
            ],
            "Resource": "*"
        }
    ]
}
```

### Create a Custom Labeling Workflow in the Console<a name="sms-security-permissions-custom-workflow"></a>

If you want to create a custom labeling workflow using the console, you must add the following actions to the policy above\.

```
{
    "Sid": "GroundTruthConsoleCustomWorkflow",
    "Effect": "Allow",
    "Action": [
        "lambda:InvokeFunction",
        "lambda:ListFunctions"
    ],
    "Resource": "*"
}
```

### Grant Permissions for Private Workforce Creation<a name="sms-security-permission-workforce-creation"></a>

When added to a permissions policy, the following permission grants access to create and manage a private workforce and work team\. To learn more about private workforces, see [Use a Private Workforce](sms-workforce-private.md)\.

```
 {
            "Effect": "Allow",
            "Action": [
                "cognito-idp:AdminAddUserToGroup",
                "cognito-idp:AdminCreateUser",
                "cognito-idp:AdminDeleteUser",
                "cognito-idp:AdminDisableUser",
                "cognito-idp:AdminEnableUser",
                "cognito-idp:AdminRemoveUserFromGroup",
                "cognito-idp:CreateGroup",
                "cognito-idp:CreateUserPool",
                "cognito-idp:CreateUserPoolClient",
                "cognito-idp:CreateUserPoolDomain",
                "cognito-idp:DescribeUserPool",
                "cognito-idp:DescribeUserPoolClient",
                "cognito-idp:List",
                "cognito-idp:UpdateUserPool",
                "cognito-idp:UpdateUserPoolClient"
                ],
            "Resource": ""
        },
```

### Grant Permissions to Subscribe to a Vendor Workforce<a name="sms-security-permissions-workforce-creation-vendor"></a>

You can add the following JSON to the policy in [Permissions Required to Use the Amazon SageMaker Ground Truth Console and API](#sms-security-permission-console-access) to grant an IAM entity permission to subscribe to a [vendor workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management-vendor.html)\.

```
{
    "Sid": "AccessAwsMarketplaceSubscriptions",
    "Effect": "Allow",
    "Action": [
        "aws-marketplace:Subscribe",
        "aws-marketplace:Unsubscribe",
        "aws-marketplace:ViewSubscriptions",
        "aws-marketplace:DescribeListings"
    ],
    "Resource": "*"
}
```

If your require more granular permissions to access SageMaker API operations than the ones described in [Permissions Required to Use the Amazon SageMaker Ground Truth Console and API](#sms-security-permission-console-access), you can add the following JSON to your policy to grant permission to list and describe the vendor work teams to which you are subscribed\.

```
{
    "Sid": "SageMakerVendorWorkteamApis",
    "Effect": "Allow",
    "Action": [
        "sagemaker:ListSubscribedWorkteams",
        "sagemaker:DescribeSubscribedWorkteam"
    ],
    "Resource": "*"
}
```

To learn more about the permissions required to use the SageMaker console, see [Using the SageMaker Console](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-console)\.

## Create an Execution Role to Start a Labeling Job<a name="sms-security-permission-execution-role"></a>

When you configure your labeling job, you need to provide an *execution role*, which is a role that SageMaker has permission to assume to start and run your labeling job\.

This role must give SageMaker permission to access the following: 
+ Amazon S3 to retrieve your input data and write output data to an Amazon S3 bucket\. You can either grant permission for an IAM role to access an entire bucket by providing the bucket ARN, or you can grant access to the role to access specific resources in a bucket\. For example, the ARN for a bucket may look similar to `arn:aws:s3:::awsexamplebucket1` and the ARN of a resource in an Amazon S3 bucket may look similar to `arn:aws:s3:::awsexamplebucket1/resource-key`\. For more information, see [Amazon S3 Resources](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-arn-format.html) in the Amazon Simple Storage Service Developer Guide\. 
+ CloudWatch to log worker metrics and labeling job statuses\.
+ AWS KMS for data encryption\. \(Optional\)
+ AWS Lambda for processing input and output data when you create a custom workflow\. 

All of these permissions can be granted with the `[AmazonSageMakerGroundTruthExecution](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerGroundTruthExecution)` managed policy *except* for data and storage volume encryption if your S3 buckets, objects, and Lambda functions do not meet the conditions specified in the policy\. 

Use the following policy examples to create an execution role that fits your specific use case\.

**Topics**
+ [Execution Role Requirements for Built\-In Task Types](#sms-security-permission-execution-role-built-in-tt)
+ [Execution Role Requirements for Custom Task Types](#sms-security-permission-execution-role-custom-tt)

### Execution Role Requirements for Built\-In Task Types<a name="sms-security-permission-execution-role-built-in-tt"></a>

The following policy grants permission to create a labeling job for a [built\-in task type](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html)\. This execution policy does not include permissions for AWS KMS data encryption or decryption\. Replace each red, italicized ARN with your own Amazon S3 ARNs\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
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
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::<input-bucket-name>/*",
                "arn:aws:s3:::<output-bucket-name>/*"
            ]
        }
    ]
}
```

If you want to create a labeling job with **automated data labeling** enabled, then you need to add the following additional policy to the execution role\. Replace `arn:aws:iam::<account-number>:role/<role-name>` with your role ARN\. You can find your IAM role ARN in the IAM console under **Roles**\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::<account-number>:role/<role-name>",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "sagemaker.amazonaws.com"
                    ]
                }
            }
        },
         {
           "Effect": "Allow", 
           "Principal": {"Service": "sagemaker.amazonaws.com" }, 
            "Action": "sts:AssumeRole" 
         }
    ]
}
```

### Execution Role Requirements for Custom Task Types<a name="sms-security-permission-execution-role-custom-tt"></a>

The following policy example can be modified used to give an execution role used in a custom labeling workflow permission to access and write to the Amazon S3 buckets used in that labeling job\. Replace each red, italicized ARN with your own Amazon S3 ARNs\. 

When you create a labeling job using a custom workflow \(custom task type\), in addition to Amazon S3 permissions, you need to add permission for AWS Lambda to process input and output data\. Modify the following policy by adding the Lambda function ARNs for your pre\- and post\-annotation lambda functions in a list under `Resource`\. 

Attach the policy to an execution role that you use to create your custom labeling workﬂow\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
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
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::<input-bucket-name>/*",
                "arn:aws:s3:::<output-bucket-name>/*"
            ]
        },
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
    ]
}
```

## Encrypt Output Data and Storage Volume with AWS KMS<a name="sms-security-kms-permissions"></a>

You can use AWS Key Management Service \(AWS KMS\) to encrypt output data from a labeling job by specifying an [AWS KMS customer managed customer master key \(CMK\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys) when you create the labeling job\. If you use the API operation `CreateLabelingJob` to create a labeling job that uses automated data labeling, you can also use a customer managed CMK to encrypt the storage volume attached to the ML compute instances to run the training and inference jobs\.

This section describes the IAM policies you must attach to your customer managed CMK to enable output data encryption and the policies you must attach to your CMK and execution role to use storage volume encryption\. To learn more about these options, see [Output Data and Storage Volume Encryption](sms-security.md)\.

### Encrypt Output Data using KMS<a name="sms-security-kms-permissions-output-data"></a>

If you specify an AWS KMS customer managed CMK to encrypt output data, you must add an IAM policy similar to the following to that key\. This policy gives the IAM execution role that you use to create your labeling job permission to use this key to perform all of the actions listed in `"Action"`\. To learn more about these actions, see [AWS KMS permissions](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the AWS Key Management Service Developer Guide\.

To use this policy, replace the IAM service\-role ARN in `"Principal"` with the ARN of the execution role you use to create the labeling job\. When you create a labeling job in the console, this is the role you specify for **IAM Role** under the **Job overview** section\. When you create a labeling job using `CreateLabelingJob`, this is ARN you specify for [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-RoleArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-RoleArn)\.

```
{
    "Sid": "AllowUseOfKmsKey",
    "Effect": "Allow",
    "Principal": {
        "AWS": "arn:aws:iam::111122223333:role/service-role/example-role"
    },
    "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
    ],
    "Resource": "*"
}
```

### Encrypt Automated Data Labeling ML Compute Instance Storage Volume<a name="sms-security-kms-permissions-storage-volume"></a>

If you specify a [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobResourceConfig.html#sagemaker-Type-LabelingJobResourceConfig-VolumeKmsKeyId](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobResourceConfig.html#sagemaker-Type-LabelingJobResourceConfig-VolumeKmsKeyId) to encrypt the storage volume attached to the ML compute instance used for automated data labeling training and inference, you must do the following:
+ Attach permissions described in [Encrypt Output Data using KMS](#sms-security-kms-permissions-output-data) to the customer managed CMK\.
+ Attach a policy similar to the following to the IAM execution role you use to create your labeling job\. This is the IAM role you specify for [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-RoleArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-RoleArn) in `CreateLabelingJob`\. To learn more about the `"kms:CreateGrant"` action that this policy permits, see [https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html) in the AWS Key Management Service API Reference\.

```
{
  "Version": "2012-10-17", 
  "Statement": 
     [  
       {
        "Effect": "Allow",
        "Action": [
           "kms:CreateGrant"
        ],
        "Resource": "*"
      }
   ]
}
```

To learn more about Ground Truth storage volume encryption, see [Use Your KMS Key to Encrypt Automated Data Labeling Storage Volume \(API Only\)](sms-security.md#sms-security-kms-storage-volume)\.