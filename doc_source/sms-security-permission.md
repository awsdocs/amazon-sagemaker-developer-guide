# Assign IAM Permissions to Use Ground Truth<a name="sms-security-permission"></a>

Use the topics on this page to learn how to use AWS Identity and Access Management \(IAM\) managed and custom policies to manage access to Ground Truth and associated resources\. 

You can use the sections on this page to learn the following: 
+ How to create IAM policies that will grant an IAM user or role permission to create a labeling job\. Administrators can use IAM policies to restrict access to Amazon SageMaker and other AWS services that are specific to Ground Truth\.
+ How to create an *execution role* for labeling jobs\. An execution role is the role that you specify when you crease a labeling job and it is used to execute your labeling job\.

The following is an overview of the topics you'll find on this page: 
+ If you are getting started using Ground Truth, or you do not require granular permissions for your use case, see [Grant General Permissions To Get Started Using Ground Truth](#sms-security-permissions-get-started)\.
+ Use the policy in [Permissions Required to Use the Amazon SageMaker Ground Truth Console](#sms-security-permission-console-access) to grant access to the Ground Truth area of the SageMaker console\. This policy includes permissions to create and modify private work teams\. To learn more about these permissions, see [Grant Permissions for Private Workforce Creation](#sms-security-permission-workforce-creation)\.
+ When you create a labeling job, you must provide an execution role\. Use [Create an Execution Role to Start a Labeling Job](#sms-security-permission-execution-role) to learn about the permissions required for this role\. 

## Grant General Permissions To Get Started Using Ground Truth<a name="sms-security-permissions-get-started"></a>

If you are getting started using Ground Truth and you do not require granular permissions for your use case, you can attach the managed policy [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) to an IAM user or role to give that entity permission to create a labeling job\. This is a broad policy that grants an IAM entity permission to use SageMaker features, as well as features of related AWS services through the console and API\. This policy will give the IAM entity permission to create a labeling job and to create and manage workforces using Amazon Cognito\. To learn more, see [AmazonSageMakerFullAccess Policy](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-amazonsagemakerfullaccess-policy)\.

To create an *execution role*, you can attach the policy [AmazonSageMakerGroundTruthExecution](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerGroundTruthExecution) to an IAM role\. An execution role is the role that you specify when you crease a labeling job and it is used to execute your labeling job\. AmazonSageMakerGroundTruthExecution grants all required permissions *except* for data and storage volume encryption if your S3 buckets, objects, and Lambda functions do not meet the conditions specified in the policy\. Additionally, be aware of the following Amazon S3 and AWS Lambda permissions included in this policy:
+ **AWS Lambda permissions** – When you create a [custom labeling workflow](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-custom-templates.html), this execution role is restricted to invoking AWS Lambda functions with one of the following strings as part of the function name: `GtRecipe`, `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\. This applies to both your pre\-annotation and post\-annotation Lambda functions\. If you choose to use names without those strings, you must explicitly provide `lambda:InvokeFunction` permission to the execution role used to create the labeling job\.
+ **Amazon S3 permissions** – This policy grants an execution role permission to access Amazon S3 buckets with the following strings in the name: `GroundTruth`, `Groundtruth`, `groundtruth`, `SageMaker`, `Sagemaker`, and `sagemaker`\. Make sure your input and output bucket names include these strings, or add additional permissions to your execution role to [grant it permission to access your S3 buckets](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_examples_s3_rw-bucket.html)\. 

**Important**  
When you create your labeling job, if you set the Task time limit \(`TaskTimeLimitInSeconds` when using the API\) to be greater than one hour \(3,600 seconds\), you must increases the *max session duration* of your execution role to be greater than or equal to the task timeout\.   
You can modify the max session duration of your execution rule using the IAM console, AWS CLI, and IAM API\. To modify your execution role, go to [Modifying a Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, select your preferred method \(console, CLI, or API\) to modify the role from the **Topics** list, and then select **Modifying a Role Maximum Session Duration** to view the instructions\. For 3D point cloud task types, refer to [Increase MaxSessionDuration for Execution Role](sms-point-cloud-general-information.md#sms-3d-pointcloud-maxsessduration)\.

## Permissions Required to Use the Amazon SageMaker Ground Truth Console<a name="sms-security-permission-console-access"></a>

To access the SageMaker console, you must have a minimum set of permissions\. To use the Ground Truth console, you need to grant permissions for additional resources\. Speciﬁcally, the console needs permissions for the AWS Marketplace to view subscriptions, Amazon S3 actions for access to your input and output ﬁles\. Amazon Cognito permission is required for initial work team setup\. 

To grant permission to an IAM user or role to use the Ground Truth area of the SageMaker console to create a labeling job, attach the following policy to the user or role\. 

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

To learn more about the permissions required to use the SageMaker console, see [Using the SageMaker Console](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-console)\.

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

When you configure your labeling job, you need to provide an *execution role* which is a role that SageMaker has permission to assume to start and run your labeling job\.

This role must give SageMaker permission to access the following: 
+ Amazon S3 to retrieve your input data and write output data to an Amazon S3 bucket\. You can either grant permission for an IAM role to access an entire bucket by providing the bucket ARN, or you can grant access to the role to access specific resources in a bucket\. For example, the ARN for a bucket may look similar to `arn:aws:s3:::awsexamplebucket1` and the ARN of a resource in an Amazon S3 bucket may look similar to `arn:aws:s3:::awsexamplebucket1/resource-key`\. For more information, see [Amazon S3 Resources](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-arn-format.html) in the Amazon Simple Storage Service Developer Guide\. 
+ CloudWatch to log worker metrics and labeling job statuses\. 
+ \(Optional\) AWS KMS for data encryption\.
+ When you create a custom labeling workflow, AWS Lambda for processing input and output data\. 

All of the permissions above can be granted with the [AmazonSageMakerGroundTruthExecution](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerGroundTruthExecution) managed policy *except* for data and storage volume encryption if your S3 buckets, objects, and Lambda functions do not meet the conditions specified in the policy\. 

When you create your labeling job, if you set the Task time limit \(`TaskTimeLimitInSeconds` when using the API\) to be greater than one hour \(3,600 seconds\), you must increases the max session duration of your execution role to be greater than or equal to the task timeout\. 

You can modify the max session duration of your execution rule using the IAM console, AWS CLI, and IAM API\. To modify your execution role, go to [Modifying a Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, select your preferred method \(console, CLI, or API\) to modify the role from the **Topics** list, and then select **Modifying a Role Maximum Session Duration** to view the instructions\. For 3D point cloud task types, refer to [Increase MaxSessionDuration for Execution Role](sms-point-cloud-general-information.md#sms-3d-pointcloud-maxsessduration)\.

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
            "arn:aws:s3:::<DOC-EXAMPLE-BUCKET1>/*",
            "arn:aws:s3:::<DOC-EXAMPLE-BUCKET2>"
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

**Important**  
You cannot use Ground Truth [automated data setup](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-data-input.html#sms-console-create-manifest-file) if you use AWS KMS to encrypt buckets or objects in Amazon S3\.

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