# Install policies and permissions<a name="scheduled-notebook-policies"></a>

Before you schedule your first notebook run, make sure that you install the proper policies and permissions\. The following instructions show you how to configure the following permissions:
+ Job execution role trust relationships
+ Additional IAM permissions attached to the job execution role
+ \(optional\) The AWS KMS permission policy to use a custom KMS key

**To modify the trust relationships**

1. Open the [IAM console](https://console.aws.amazon.com/iam/)\.

1. Select **Roles** in the left panel\.

1. Find the job execution role for your notebook job and choose the role name\. 

1. Choose the **Trust relationships** tab\.

1. Choose **Edit trust policy**\.

1. Copy and paste the following policy:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": {
                   "Service": "sagemaker.amazonaws.com"
               },
               "Action": "sts:AssumeRole"
           },
           {
               "Effect": "Allow",
               "Principal": {
                   "Service": "events.amazonaws.com"
               },
               "Action": "sts:AssumeRole"
           }
       ]
   }
   ```

1. Choose **Update Policy**\.

**Add additional IAM permissions**

The following JSON snippet is an example policy that you should add to the Studio execution and notebook job roles if you don’t use the Studio execution role as the notebook job role\. Review and modify this policy if you need to further restrict privileges\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":"iam:PassRole",
         "Resource":"arn:aws:iam::*:role/*",
         "Condition":{
            "StringLike":{
               "iam:PassedToService":[
                  "sagemaker.amazonaws.com",
                  "events.amazonaws.com"
               ]
            }
         }
      },
      {
         "Effect":"Allow",
         "Action":[
            "events:TagResource",
            "events:DeleteRule",
            "events:PutTargets",
            "events:DescribeRule",
            "events:PutRule",
            "events:RemoveTargets",
            "events:DisableRule",
            "events:EnableRule"
         ],
         "Resource":"*",
         "Condition":{
            "StringEquals":{
               "aws:ResourceTag/sagemaker:is-scheduling-notebook-job":"true"
            }
         }
      },
      {
         "Effect":"Allow",
         "Action":[
            "s3:CreateBucket",
            "s3:PutBucketVersioning",
            "s3:PutEncryptionConfiguration"
         ],
         "Resource":"arn:aws:s3:::sagemaker-automated-execution-*"
      },
      {
         "Effect":"Allow",
         "Action":[
            "sagemaker:ListTags"
         ],
         "Resource":[
            "arn:aws:sagemaker:*:*:user-profile/*",
            "arn:aws:sagemaker:*:*:space/*",
            "arn:aws:sagemaker:*:*:training-job/*",
            "arn:aws:sagemaker:*:*:pipeline/*"
         ]
      },
      {
         "Effect":"Allow",
         "Action":[
            "sagemaker:AddTags"
         ],
         "Resource":[
            "arn:aws:sagemaker:*:*:training-job/*",
            "arn:aws:sagemaker:*:*:pipeline/*"
         ]
      },
      {
         "Effect":"Allow",
         "Action":[
            "ec2:CreateNetworkInterface",
            "ec2:CreateNetworkInterfacePermission",
            "ec2:CreateVpcEndpoint",
            "ec2:DeleteNetworkInterface",
            "ec2:DeleteNetworkInterfacePermission",
            "ec2:DescribeDhcpOptions",
            "ec2:DescribeNetworkInterfaces",
            "ec2:DescribeRouteTables",
            "ec2:DescribeSecurityGroups",
            "ec2:DescribeSubnets",
            "ec2:DescribeVpcEndpoints",
            "ec2:DescribeVpcs",
            "ecr:BatchCheckLayerAvailability",
            "ecr:BatchGetImage",
            "ecr:GetDownloadUrlForLayer",
            "ecr:GetAuthorizationToken",
            "s3:ListBucket",
            "s3:GetBucketLocation",
            "s3:GetEncryptionConfiguration",
            "s3:PutObject",
            "s3:DeleteObject",
            "s3:GetObject",
            "sagemaker:DescribeDomain",
            "sagemaker:DescribeUserProfile",
            "sagemaker:DescribeSpace",
            "sagemaker:DescribeStudioLifecycleConfig",
            "sagemaker:DescribeImageVersion",
            "sagemaker:DescribeAppImageConfig",
            "sagemaker:CreateTrainingJob",
            "sagemaker:DescribeTrainingJob",
            "sagemaker:StopTrainingJob",
            "sagemaker:Search",
            "sagemaker:CreatePipeline",
            "sagemaker:DescribePipeline",
            "sagemaker:DeletePipeline",
            "sagemaker:StartPipelineExecution"
         ],
         "Resource":"*"
      }
   ]
}
```

**AWS KMS permission policy \(optional\)**

By default, Studio encrypts the input and output Amazon S3 buckets using server side encryption, but you can specify a custom KMS key to encrypt your data in the output S3 bucket and the storage volume attached to the notebook job\.

If you want to use a custom KMS key, attach the following policy and supply your own KMS key ARN\.

```
{
    "Version": "2012-10-17",
    "Statement": [
      {
         "Effect":"Allow",
         "Action":[
            "kms:Encrypt",
            "kms:Decrypt",
            "kms:ReEncrypt*",
            "kms:GenerateDataKey*",
            "kms:DescribeKey",
            "kms:CreateGrant"
         ],
         "Resource":"your_KMS_key_ARN"
      }
   ]
}
```