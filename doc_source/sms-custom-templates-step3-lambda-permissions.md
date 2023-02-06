# Required Permissions To Use AWS Lambda With Ground Truth<a name="sms-custom-templates-step3-lambda-permissions"></a>

You may need to configure some or all the following to create and use AWS Lambda with Ground Truth\. 
+ You need to grant an IAM role or user \(collectively, an IAM entity\) permission to create the pre\-annotation and post\-annotation Lambda functions using AWS Lambda, and to choose them when creating the labeling job\. 
+ The IAM execution role specified when the labeling job is configured needs permission to invoke the pre\-annotation and post\-annotation Lambda functions\. 
+ The post\-annotation Lambda functions may need permission to access Amazon S3\.

Use the following sections to learn how to create the IAM entities and grant permissions described above\.

**Topics**
+ [Grant Permission to Create and Select an AWS Lambda Function](#sms-custom-templates-step3-postlambda-create-perms)
+ [Grant IAM Execution Role Permission to Invoke AWS Lambda Functions](#sms-custom-templates-step3-postlambda-execution-role-perms)
+ [Grant Post\-Annotation Lambda Permissions to Access Annotation](#sms-custom-templates-step3-postlambda-perms)

## Grant Permission to Create and Select an AWS Lambda Function<a name="sms-custom-templates-step3-postlambda-create-perms"></a>

If you do not require granular permissions to develop pre\-annotation and post\-annotation Lambda functions, you can attach the AWS managed policy `AWSLambda_FullAccess` to a user or role\. This policy grants broad permissions to use all Lambda features, as well as permission to perform actions in other AWS services with which Lambda interacts\.

To create a more granular policy for security\-sensitive use cases, refer to the documentation [Identity\-based IAM policies for Lambda](https://docs.aws.amazon.com/lambda/latest/dg/access-control-identity-based.html) in the to AWS Lambda Developer Guide to learn how to create an IAM policy that fits your use case\. 

**Policies to Use the Lambda Console**

If you want to grant an IAM entity permission to use the Lambda console, see [Using the Lambda console](https://docs.aws.amazon.com/lambda/latest/dg/security_iam_id-based-policy-examples.html#security_iam_id-based-policy-examples-console) in the AWS Lambda Developer Guide\.

Additionally, if you want the user to be able to access and deploy the Ground Truth starter pre\-annotation and post\-annotation functions using the AWS Serverless Application Repository in the Lambda console, you must specify the *`<aws-region>`* where you want to deploy the functions \(this should be the same AWS Region used to create the labeling job\), and add the following policy to the IAM role\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "serverlessrepo:ListApplicationVersions",
                "serverlessrepo:GetApplication",
                "serverlessrepo:CreateCloudFormationTemplate"
            ],
            "Resource": "arn:aws:serverlessrepo:<aws-region>:838997950401:applications/aws-sagemaker-ground-truth-recipe"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "serverlessrepo:SearchApplications",
            "Resource": "*"
        }
    ]
}
```

**Policies to See Lambda Functions in the Ground Truth Console**

To grant an IAM entity permission to view Lambda functions in the Ground Truth console when the user is creating a custom labeling job, the entity must have the permissions described in [Grant IAM Permission to Use the Amazon SageMaker Ground Truth Console](sms-security-permission-console-access.md), including the permissions described in the section [Custom Labeling Workflow Permissions](sms-security-permission-console-access.md#sms-security-permissions-custom-workflow)\.

## Grant IAM Execution Role Permission to Invoke AWS Lambda Functions<a name="sms-custom-templates-step3-postlambda-execution-role-perms"></a>

If you add the IAM managed policy [AmazonSageMakerGroundTruthExecution](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerGroundTruthExecution) to the IAM execution role used to create the labeling job, this role has permission to list and invoke Lambda functions with one of the following strings in the function name: `GtRecipe`, `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\. 

If the pre\-annotation or post\-annotation Lambda function names do not include one of the terms in the preceding paragraph, or if you require more granular permission than those in the `AmazonSageMakerGroundTruthExecution` managed policy, you can add a policy similar to the following to give the execution role permission to invoke pre\-annotation and post\-annotation functions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": 
                "lambda:InvokeFunction",
            "Resource": [
                "arn:aws:lambda:<region>:<account-id>:function:<pre-annotation-lambda-name>",
                "arn:aws:lambda:<region>:<account-id>:function:<post-annotation-lambda-name>"
            ]
        }
    ]
}
```

## Grant Post\-Annotation Lambda Permissions to Access Annotation<a name="sms-custom-templates-step3-postlambda-perms"></a>

As described in [Post\-annotation Lambda](sms-custom-templates-step3-lambda-requirements.md#sms-custom-templates-step3-postlambda), the post\-annotation Lambda request includes the location of the annotation data in Amazon S3\. This location is identified by the `s3Uri` string in the `payload` object\. To process the annotations as they come in, even for a simple pass through function, you need to assign the necessary permissions to the post\-annotation [Lambda execution role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html) to read files from the Amazon S3\.

There are many ways that you can configure your Lambda to access annotation data in Amazon S3\. Two common ways are:
+ Allow the Lambda execution role to assume the SageMaker execution role identified in `roleArn` in the post\-annotation Lambda request\. This SageMaker execution role is the one used to create the labeling job, and has access to the Amazon S3 output bucket where the annotation data is stored\.
+ Grant the Lambda execution role permission to access the Amazon S3 output bucket directly\.

Use the following sections to learn how to configure these options\. 

**Grant Lambda Permission to Assume SageMaker Execution Role**

To allow a Lambda function to assume a SageMaker execution role, you must attach a policy to the Lambda function's execution role, and modify the trust relationship of the SageMaker execution role to allow Lambda to assume it\.

1. [Attach the following IAM policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) to your Lambda function's execution role to assume the SageMaker execution role identified in `Resource`\. Replace `222222222222` with an [AWS account ID](https://docs.aws.amazon.com/general/latest/gr/acct-identifiers.html)\. Replace `sm-execution-role` with the name of the assumed role\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": {
           "Effect": "Allow",
           "Action": "sts:AssumeRole",
           "Resource": "arn:aws:iam::222222222222:role/sm-execution-role"
       }
   }
   ```

1. [Modify the trust policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-managingrole_edit-trust-policy) of the SageMaker execution role to include the following `Statement`\. Replace `222222222222` with an [AWS account ID](https://docs.aws.amazon.com/general/latest/gr/acct-identifiers.html)\. Replace `my-lambda-execution-role` with the name of the assumed role\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": {
                   "AWS": "arn:aws:iam::222222222222:role/my-lambda-execution-role"
               },
               "Action": "sts:AssumeRole"
           }
       ]
   }
   ```

**Grant Lambda Execution Role Permission to Access S3**

You can add a policy similar to the following to the post\-annotation Lambda function execution role to give it S3 read permissions\. Replace **DOC\-EXAMPLE\-BUCKET** with the name of the output bucket you specify when you create a labeling job\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*"
        }
    ]
}
```

To add S3 read permissions to a Lambda execution role in the Lambda console, use the following procedure\. 

**Add S3 read permissions to post\-annotation Lambda:**

1. Open the [**Functions** page](https://console.aws.amazon.com/lambda/home#/functions) in the Lambda console\.

1. Choose the name of the post\-annotation function\.

1. Choose **Configuration** and then choose **Permissions**\.

1. Select the **Role name** and the summary page for that role opens in the IAM console in a new tab\. 

1. Select **Attach policies**\.

1. Do one of the following:
   + Search for and select **`AmazonS3ReadOnlyAccess`** to give the function permission to read all buckets and objects in the account\. 
   + If you require more granular permissions, select **Create policy** and use the policy example in the preceding section to create a policy\. Note that you must navigate back to the execution role summary page after you create the policy\.

1. If you used the `AmazonS3ReadOnlyAccess` managed policy, select **Attach policy**\. 

   If you created a new policy, navigate back to the Lambda execution role summary page and attach the policy you just created\.