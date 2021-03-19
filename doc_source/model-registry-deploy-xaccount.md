# Deploy a Model Version from a Different Account<a name="model-registry-deploy-xaccount"></a>

Enable an AWS account to deploy model versions that were created in a different account by adding a cross\-account resource policy\. For example, one team in your organization might be responsible for training models, and a different team is responsible for deploying and updating models\. When you create resource policies, you apply the policy to the resource to which you want to grant access\. For more information about cross\-account resource policies in AWS, see [Cross\-account policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic-cross-account.html) in the *AWS Identity and Access Management User Guide*\.

To enable cross\-account model deployment in SageMaker, you have to provide a cross\-account resource policy for the model group that contains the model versions you want to deploy, the Amazon ECR repository where the inference image for the model group resides, and the Amazon S3 bucket where the model versions are stored\. The following example creates cross\-account policies for all three of these resources, and applies the policies to the resources\.

```
import json

# cross account id to grant access to
cross_account_id = "123456789012"

# 1. Create policy for access to the ECR repository
ecr_repository_policy = {
    'Version': '2012-10-17',
    'Statement': [{
        'Sid': 'AddPerm',
        'Effect': 'Allow',
        'Principal': {
            'AWS': f'arn:aws:iam::{cross_account_id}:root'
        },
        'Action': ['ecr:*']
    }]
}

# Convert the ECR policy from JSON dict to string
ecr_repository_policy = json.dumps(ecr_repository_policy)

# Set the new ECR policy
ecr = boto3.client('ecr')
respose = ecr.set_repository_policy(
    registryId = account,
    repositoryName = 'decision-trees-sample',
    policyText = ecr_repository_policy
)

# 2. Create policy for access to the S3 bucket
bucket_policy = {
    'Version': '2012-10-17',
    'Statement': [{
        'Sid': 'AddPerm',
        'Effect': 'Allow',
        'Principal': {
            'AWS': f'arn:aws:iam::{cross_account_id}:root'
        },
        'Action': 's3:*',
        'Resource': f'arn:aws:s3:::{bucket}/*'
    }]
}

# Convert the policy from JSON dict to string
bucket_policy = json.dumps(bucket_policy)

# Set the new policy
s3 = boto3.client('s3')
respose = s3.put_bucket_policy(
    Bucket = bucket, 
    Policy = bucket_policy)

# 3. Create policy for access to the ModelPackageGroup
model_pacakge_group_policy = {
    'Version': '2012-10-17',
    'Statement': [{
        'Sid': 'AddPermModelPackageGroup',
        'Effect': 'Allow',
        'Principal': {
            'AWS': f'arn:aws:iam::{cross_account_id}:root'
        },
        'Action': ['sagemaker:DescribeModelPackageGroup'],
        'Resource': f'arn:aws:sagemaker:{region}:{account}:model-package-group/{model_package_group_name}'
    },{
        'Sid': 'AddPermModelPackageVersion',
        'Effect': 'Allow',
        'Principal': {
            'AWS': f'arn:aws:iam::{cross_account_id}:root'
        },
        'Action': ["sagemaker:DescribeModelPackage",
                   "sagemaker:ListModelPackages",
                   "sagemaker:UpdateModelPackage",
                   "sagemaker:CreateModel"],
        'Resource': f'arn:aws:sagemaker:{region}:{account}:model-package/{model_package_group_name}/*'
    }]
}

# Convert the policy from JSON dict to string
model_pacakge_group_policy = json.dumps(model_pacakge_group_policy)

# Set the new policy
respose = sm_client.put_model_package_group_policy(
    ModelPackageGroupName = model_package_group_name, 
    ResourcePolicy = model_pacakge_group_policy)

print('ModelPackageGroupArn : {}'.format(create_model_pacakge_group_response['ModelPackageGroupArn']))
print("First Versioned ModelPackageArn: " + model_package_arn)
print("Second Versioned ModelPackageArn: " + model_package_arn2)

print("Success! You are all set to proceed for cross account deployment.")
```

The example assumes that you previously defined the following variables:
+ `account` \- The account of the authenticated caller\.
+ `bucket` \- The S3 bucket where the model versions are stored\.
+ `sm_client` \- A SageMaker Boto3 client\.
+ `model_package_group_name` \- The model group to which you want to grant access\.

To be able to deploy a model that was created in a different account, the user must have a role that has access to SageMaker actions, such as a role with the `AmazonSageMakerFullAccess` managed policy\. For information about SageMaker managed policies, see [AWS Managed \(Predefined\) Policies for Amazon SageMaker](access-policy-aws-managed-policies.md)\.