# Setting up Hyperparameter Optimization<a name="hpo-setup"></a>

## Create an IAM Role<a name="hpo-auth"></a>

To start Hyperparameter Optimization \(HPO\) jobs, add permissions to your Amazon SageMaker execution role with the required permsissions\.

For more information about IAM roles, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

**To add required permissions for HPO to your IAM execution role**

1. Sign in to the AWS Management Console and open the IAM console at https://console\.aws\.amazon\.com/iam/\. 

1. In the navigation pane of the IAM console, choose **Roles**\.

1. In the **Role name** list, choose the name that begins with `AmazonSageMaker-ExecutionRole`\.

1. In the **Policy name** list, choose the policy that begins with `AmazonSageMaker-ExecutionPolicy`\.

1. Choose **Edit policy**\.

1. Choose **JSON**\.

1. Edit the policy by adding the line `"sagemakerhpo:*",` to the`Action` element:

   The policy should look as follows:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "sagemaker:*",
                   "sagemakerhpo:*",
                   "ecr:GetAuthorizationToken",
                   "ecr:GetDownloadUrlForLayer",
                   "ecr:BatchGetImage",
                   "ecr:BatchCheckLayerAvailability",
                   "cloudwatch:*",
                   "logs:*",
                   "s3:CreateBucket",
                   "s3:ListBucket",
                   "s3:GetBucketLocation",
                   "s3:GetObject",
                   "s3:PutObject",
                   "s3:DeleteObject",
                   "s3:ListAllMyBuckets"
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "iam:PassRole"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. Choose **Save changes**\.

## Setting up Hyperparameter Optimization for the AWS CLI<a name="hpo-cli"></a>

You can install Hyperparameter Optimization \(HPO\) libraries to use with the AWS Command Line Interface\.

**To install HPO libraries**

1. Ensure that you have `Boto 3` installed\. For information about installing `Boto 3`, see [Boto 3 Quickstart](http://boto3.readthedocs.io/en/latest/guide/quickstart.html)\.

1. Ensure that you have the AWS Command Line Interface installed\. For information about installing the AWS CLI, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\.

1. Run the following Linux command to download the HPO libraries:

   ```
   curl $(aws s3 presign s3://sagemakerhpo/setup/setup-notebook-instance.sh) | bash
   ```

1. Run the following AWS CLI command to configure HPO:

   ```
   aws configure add-model --service-model file://awsmodel/sagemakerhpo-2017-11-08.normal.json --service-name
   ```

1. Run the following AWS CLI command to ensure that HPO is properly configured>:

   ```
   aws sagemakerhpo help
   ```

## Setting up Hyperparameter Optimization for Amazon SageMaker Notebooks<a name="hpo-notebook"></a>

You can use Hyperparameter Optimization in Amazon SageMaker notebook instances\. For information about creating and starting Amazon SageMaker notebook instances, see [Notebook Instances and Notebooks](how-it-works-notebooks-instances.md)\.

**Important**  
To use HPO, you must create and start a notebook instance in the `us-west-2` region\. 

**To set up HPO in Amazon SageMaker Notebooks**

1. Start a Amazon SageMaker notebook instance\.

1. Create a new notebook and name it "Setup"\.

1. Run the following command inside the new notebook:

   ```
   !curl $(aws s3 presign s3://sagemakerhpo/setup/setup-notebook-instance.sh) | bash
   ```

## Setting up the smhpolib Library<a name="hpo-smhpolib"></a>