# Grant IAM Permission to Use the Amazon SageMaker Ground Truth Console<a name="sms-security-permission-console-access"></a>

To use the Ground Truth area of the SageMaker console, you need to grant permission to an IAM entity to access SageMaker and other AWS services that Ground Truth interacts with\. Required permissions to access other AWS services depends on your use\-case: 
+ Amazon S3 permissions are required for all use cases\. These permissions must grant access to the Amazon S3 buckets that contain input and output data\. 
+ AWS Marketplace permissions are required to use a vendor workforce\.
+ Amazon Cognito permission are required for private work team setup\.
+ AWS KMS permissions are required to view available AWS KMS keys that can be used for output data encryption\.
+ IAM permissions are required to either list pre\-existing execution roles, or to create a new one\. Additionally, you must use add a `PassRole` permission to allow SageMaker to use the execution role chosen to start the labeling job\.

The following sections list policies you may want to grant to an IAM role to use one or more functions of Ground Truth\. 

**Topics**
+ [Ground Truth Console Permissions](#sms-security-permissions-console-all)
+ [Custom Labeling Workflow Permissions](#sms-security-permissions-custom-workflow)
+ [Private Workforce Permissions](#sms-security-permission-workforce-creation)
+ [Vendor Workforce Permissions](#sms-security-permissions-workforce-creation-vendor)

## Ground Truth Console Permissions<a name="sms-security-permissions-console-all"></a>

To grant permission to an IAM user or role to use the Ground Truth area of the SageMaker console to create a labeling job, attach the following policy to the user or role\. The following policy will give an IAM role permission to create a labeling job using a [built\-in task type](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html) task type\. If you want to create a custom labeling workflow, add the policy in [Custom Labeling Workflow Permissions](#sms-security-permissions-custom-workflow) to the following policy\. Each `Statement` included in the following policy is described below this code block\.

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
                "aws-marketplace:ViewSubscriptions"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SecretsManager",
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

This policy includes the following statements\. You can scope down any of these statements by adding specific resourses to the `Resource` list for that statement\.

`SageMakerApis`

This statement includes `sagemaker:*`, which allows the user to perform all [SageMaker API actions](sagemaker/latest/APIReference/API_Operations.html)\. You can reduce the scope of this policy by restricting users from performing actions that are not used to create and monitoring a labeling job\. 

**`KmsKeysForCreateForms`**

You only need to include this statement if you want to grant a user permission to list and select AWS KMS keys in the Ground Truth console to use for output data encryption\. The policy above grants a user permission to list and select any key in the account in AWS KMS\. To restrict the keys that a user can list and select, specify those key ARNs in `Resource`\.

**`SecretsManager`**

This statement gives the user permission to describe, list, and create resources in AWS Secrets Manager required to create the labeling job\.

`ListAndCreateExecutionRoles`

This statement gives a user permission to list \(`ListRoles`\) and create \(`CreateRole`\) IAM roles in your account\. It also grants the user permission to create \(`CreatePolicy`\) policies and attach \(`AttachRolePolicy`\) policies to IAM entities\. These are required to list, select, and if required, create an execution role in the console\. 

If you have already created an execution role, and want to narrow the scope of this statement so that users can only select that role in the console, specify the ARNs of the IAM roles you want the user to have permission to view in `Resource` and remove the actions `CreateRole`, `CreatePolicy`, and `AttachRolePolicy`\.

`AccessAwsMarketplaceSubscriptions`

These permissions are required to view and choose vendor work teams that you are already subscribed to when creating a labeling job\. To give the user permission to *subscribe* to vendor work teams, add the statement in [Vendor Workforce Permissions](#sms-security-permissions-workforce-creation-vendor) to the policy above

`PassRoleForExecutionRoles`

This is required to give the labeling job creator permission to preview the worker UI and verify that input data, labels, and instructions display correctly\. This statement gives an IAM entity permissions to pass the IAM execution role used to create the labeling job to SageMaker to render and preview the worker UI\. To narrow the scope of this policy, add the role ARN of the execution role used to create the labeling job under `Resource`\.

**`GroundTruthConsole`**
+ `groundtruthlabeling` – This allows a user to perform actions required to use certain features of the Ground Truth console\. These include permissions to describe the labeling job status \(`DescribeConsoleJob`\), list all dataset objects in the input manifest file \(`ListDatasetObjects`\), filter the dataset if dataset sampling is selected \(`RunFilterOrSampleDatasetJob`\), and to generate input manifest files if automated data labeling is used \(`RunGenerateManifestByCrawlingJob`\)\. These actions are only available when using the Ground Truth console and cannot be called directly using an API\.
+ `lambda:InvokeFunction` and `lambda:ListFunctions` – these actions give users permission to list and invoke Lambda functions that are used to run a custom labeling workflow\.
+ `s3:*` – All Amazon S3 permissions included in this statement are used to view Amazon S3 buckets for [automated data setup](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-console-create-manifest-file.html) \(`ListAllMyBuckets`\), access input data in Amazon S3 \(`ListBucket`, `GetObject`\), check for and create a CORS policy in Amazon S3 if needed \(`GetBucketCors` and `PutBucketCors`\), and write labeling job output files to S3 \(`PutObject`\)\.
+ `cognito-idp` – These permissions are used to create, view and manage and private workforce using Amazon Cognito\. To learn more about these actions, refer to the [Amazon Cognito API References](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-reference.html)\.

## Custom Labeling Workflow Permissions<a name="sms-security-permissions-custom-workflow"></a>

Add the following statement to a policy similar to the one in [Ground Truth Console Permissions](#sms-security-permissions-console-all) to give an IAM user permission to select pre\-existing pre\-annotation and post\-annotation Lambda functions while [creating a custom labeling workflow](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-custom-templates.html)\.

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

To learn how to give an IAM entity permission to create and test pre\-annotation and post\-annotation Lambda functions, see [Required Permissions To Use Lambda With Ground Truth](http://docs.aws.amazon.com/sagemaker/latest/dg/sms-custom-templates-step3-lambda-permissions.html)\.

## Private Workforce Permissions<a name="sms-security-permission-workforce-creation"></a>

When added to a permissions policy, the following permission grants access to create and manage a private workforce and work team using Amazon Cognito\. These permissions are not required to use an [OIDC IdP workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-create-private-oidc.html#sms-workforce-create-private-oidc-next-steps)\.

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
        "cognito-idp:List*",
        "cognito-idp:UpdateUserPool",
        "cognito-idp:UpdateUserPoolClient"
        ],
    "Resource": "*"
}
```

To learn more about creating private workforce using Amazon Cognito, see [Create and Manage Amazon Cognito Workforce](sms-workforce-private-use-cognito.md)\. 

## Vendor Workforce Permissions<a name="sms-security-permissions-workforce-creation-vendor"></a>

You can add the following statement to the policy in [Grant IAM Permission to Use the Amazon SageMaker Ground Truth Console](#sms-security-permission-console-access) to grant an IAM entity permission to subscribe to a [vendor workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management-vendor.html)\.

```
{
    "Sid": "AccessAwsMarketplaceSubscriptions",
    "Effect": "Allow",
    "Action": [
        "aws-marketplace:Subscribe",
        "aws-marketplace:Unsubscribe",
        "aws-marketplace:ViewSubscriptions"
    ],
    "Resource": "*"
}
```
