# Step 1: Before You Begin<a name="sms-getting-started-step1"></a>

Before you begin using the SageMaker console to create a labeling job, you must set up the dataset for use\. Do this:

1. Save two images at publicly available HTTP URLs\. The images are used when creating instructions for completing a labeling task\. The images should have an aspect ratio of around 2:1\. For this exercise, the content of the images is not important\.

1. Create an Amazon S3 bucket to hold the input and output files\. The bucket must be in the same Region where you are running Ground Truth\. Make a note of the bucket name because you use it during step 2\.

   Ground Truth requires all S3 buckets that contain labeling job input image data have a CORS policy attached\. To learn more about this change, see [CORS Permission Requirement](sms-cors-update.md)\.

1. You can create an IAM role or let SageMaker create a role with the [AmazonSageMakerFullAccess](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam-awsmanpol.html#security-iam-awsmanpol-AmazonSageMakerFullAccess) IAM policy\. Refer to [Creating IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) and assign the following permissions policy to the user that is creating the labeling job:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "sagemakergroundtruth",
               "Effect": "Allow",
               "Action": [
                   "cognito-idp:CreateGroup",
                   "cognito-idp:CreateUserPool",
                   "cognito-idp:CreateUserPoolDomain",
                   "cognito-idp:AdminCreateUser",
                   "cognito-idp:CreateUserPoolClient",
                   "cognito-idp:AdminAddUserToGroup",
                   "cognito-idp:DescribeUserPoolClient",
                   "cognito-idp:DescribeUserPool",
                   "cognito-idp:UpdateUserPool"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

## Next<a name="step1-next"></a>

[Step 2: Create a Labeling Job](sms-getting-started-step2.md)