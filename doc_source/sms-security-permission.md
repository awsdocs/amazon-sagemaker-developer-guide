# Assign IAM Permissions to Use Ground Truth<a name="sms-security-permission"></a>

Use this topic to learn how to create an AWS Identity and Access Management \(IAM\) user or role with required permissions to create a labeling job and how to create an *execution role* for labeling jobs\. Administrators can use these policies to restrict access to services that are specific to Ground Truth\. 

If you are new to Ground Truth and do not require the granular permissions described here, you can use the AWS managed policy, AmazonSageMakerFullAccess, to grant access to an IAM entity to create a labeling job\. You can also attach AmazonSageMakerFullAccess to an IAM role to create an execution role\. To learn more about this managed policy, see [AmazonSageMakerFullAccess Policy](sagemaker-roles.md#sagemaker-roles-amazonsagemakerfullaccess-policy)\. 

**Important**  
When you create a custom labeling workflow, the AmazonSageMakerFullAccess policy is restricted to invoking AWS Lambda functions with one of the following four strings as part of the function name: `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\. This applies to both your pre\-annotation and post\-annotation Lambda functions\. If you choose to use names without those strings, you must explicitly provide `lambda:InvokeFunction` permission to the IAM role used to create the labeling job\.

The following is an overview of the topics you'll find on this page: 
+ Use the policy in [Permissions Required to Use the Amazon SageMaker Ground Truth Console](#sms-security-permission-console-access) to grant access to the Ground Truth area of the Amazon SageMaker console\. This policy includes permissions to create and modify private work teams\. To learn more about these permissions, see [Grant Permissions for Private Workforce Creation](#sms-security-permission-workforce-creation)\.
+ When you create a labeling job, you must provide an execution role\. Use [Create an Execution Role to Start a Labeling Job](#sms-security-permission-execution-role) to learn about the permissions required for this role\. 

For more information about IAM users and roles, see [Identities \(Users, Groups, and Roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the IAM User Guide\. 

## Permissions Required to Use the Amazon SageMaker Ground Truth Console<a name="sms-security-permission-console-access"></a>

To access the Amazon SageMaker console, you must have a minimum set of permissions\. To use the Ground Truth console, you need to grant permissions for additional resources\. Speciﬁcally, the console needs permissions for the AWS Marketplace to view subscriptions, Amazon S3 actions for access to your input and output ﬁles\. Amazon Cognito permission is required for initial work team setup\. 

To grant permission to an IAM user or role to use the Ground Truth area of the Amazon SageMaker console to create a labeling job, attach the following policy to the user or role\. 

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

To learn more about the permissions required to use the Amazon SageMaker console, see [Using the Amazon SageMaker Console](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-console)\.

## Grant Permissions for Private Workforce Creation<a name="sms-security-permission-workforce-creation"></a>

When added to a permissions policy, the following permission grants access to create and manage a private workforce and workteam\. To learn more about private workforces, see [Use a Private Workforce](sms-workforce-private.md)\.

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

## Create an Execution Role to Start a Labeling Job<a name="sms-security-permission-execution-role"></a>

When you configure your labeling job, you need to provide an *execution role* which is a role that Amazon SageMaker has permission to assume to start and run your labeling job\.

This role must give Amazon SageMaker permission to access the following: 
+ Amazon S3 to retrieve your input data and write output data to an Amazon S3 bucket\. You can either grant permission for an IAM role to access an entire bucket by providing the bucket ARN, or you can grant access to the role to access specific resources in a bucket\. For example, the ARN for a bucket may look similar to `arn:aws:s3:::awsexamplebucket1` and the ARN of a resource in an Amazon S3 bucket may look similar to `arn:aws:s3:::awsexamplebucket1/resource-key`\. For more information, see [Amazon S3 Resources](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-arn-format.html) in the Amazon Simple Storage Service Developer Guide\. 
+ CloudWatch to log worker metrics and labeling job statuses\. 
+ \(Optional\) AWS KMS for data encryption\.
+ When you create a custom labeling workflow, AWS Lambda for processing input and output data\. 

All of the permissions above can be granted with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) managed policy *except* for data and storage volume encryption if your S3 buckets, objects, and Lambda functions meet the conditions specified in the policy\. 

Use the following policy examples to create an execution role that fits your specific use case\. 

The following policy grants permission to create a labeling job for a built\-in task type\. This execution policy does not include permissions for AWS KMS data encryption or decryption\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
    {
       "Effect": "Allow", 
        "Principal": {"Service": "sagemaker.amazonaws.com" }, 
        "Action": "sts:AssumeRole" 
    },
    {
       "Effect": "Allow",
       "Action": [
            "iam:PassRole"
       ],
       "Resource": "arn:{customer role arn}",
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
       "Action": [
            "s3:GetObject",
            "s3:PutObject",
            "s3:GetBucketLocation"
        ],
        "Resource": [
            "arn:aws:s3:::<AWSDOC-EXAMPLE-BUCKET1>/*",
            "arn:aws:s3:::<AWSDOC-EXAMPLE-BUCKET2>"
            ]
     },
     
     {
        "Effect": "Allow", 
          "Action": [ 
              "cloudwatch:PutMetricData",
              "logs:CreateLogStream",
              "logs:PutLogEvents", 
              "logs:CreateLogGroup", 
              "logs:DescribeLogStreams", 
              ], 
            "Resource": "*" 
      }
   ]
}
```

You can use the following policy to create an execution role that works with an automated labeling job\. Replace `arn:aws:iam::<account-number>:role/<role-name>` with your role ARN\. You can find your IAM role ARN in the IAM console under **Roles**\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:GetBucketLocation",
                "s3:ListBucket"
            ],
            "Resource": "*"
        },
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

To create a custom labeling workflow, you need to add permission for AWS Lambda to process input and output data\. Modify the following policy by adding the Lambda function ARNs for your pre\- and post\-annotation lambda functions in a list under `Resource`\. Attach the policy to an execution role that you use to create custom labeling workﬂow\.

```
 {
    "Version": "2012-10-17", 
    "Statement": 
       [  
         {
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

To add input data decryption or output data encryption using AWS KMS to any type of labeling job, modify the following policy by listing KMS key ARNs you want to grant permissions to use under `Resource`\. Attach the policy to an execution role\. 

```
{
  "Version": "2012-10-17", 
  "Statement": 
     [  
        {
        "Effect": "Allow",
         "Action": [
                "kms:DescribeKey",
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:ListAliases"
            ],
         "Resource": [
                "arn:aws:kms:<region>:<account-id>:key/<key-id>"
                "arn:aws:kms:<region>:<account-id>:key/<key-id>"
                
         ]
       }
    ]
}
```

To encrypt the storage volume attached to the ML compute instances that run the training job for automated labeling, include the following in one of the execution policies above in the `Statement` section\. Storage volume encryption is only available when you create a labeling job using the API operation [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. For more information, see `[LabelingJobResourceConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobResourceConfig.html#sagemaker-Type-LabelingJobResourceConfig-VolumeKmsKeyId)`\. 

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