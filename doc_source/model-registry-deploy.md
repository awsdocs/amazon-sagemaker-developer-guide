# Deploy a Model in the Registry<a name="model-registry-deploy"></a>

After you register a model version and approve it for deployment, deploy it to a SageMaker endpoint for real\-time inference\.

When you create an MLOps project and choose an MLOps project template that includes model deployment, approved model versions in the model registry are automatically deployed to production\. For information about using SageMaker MLOps projects, see [Automate MLOps with SageMaker Projects](sagemaker-projects.md)\.

You can also enable an AWS account to deploy model versions that were created in a different account by adding a cross\-account resource policy\. For example, one team in your organization might be responsible for training models, and a different team is responsible for deploying and updating models\.

**Topics**
+ [Deploy a Model in the Registry \(SageMaker SDK\)](#model-registry-deploy-smsdk)
+ [Deploy a Model in the Registry \(Boto3\)](#model-registry-deploy-api)
+ [Deploy a Model Version from a Different Account](#model-registry-deploy-xaccount)

## Deploy a Model in the Registry \(SageMaker SDK\)<a name="model-registry-deploy-smsdk"></a>

To deploy a model version using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) use the following code snippet:

```
from sagemaker import ModelPackage
from time import gmtime, strftime

model_package_arn = 'arn:aws:sagemaker:us-east-2:12345678901:model-package/modeltest/1'
model = ModelPackage(role=role, 
                     model_package_arn=model_package_arn, 
                     sagemaker_session=sagemaker_session)
model.deploy(initial_instance_count=1, instance_type='ml.m5.xlarge')
```

## Deploy a Model in the Registry \(Boto3\)<a name="model-registry-deploy-api"></a>

To deploy a model version using the AWS SDK for Python \(Boto3\), complete the following steps:

1. Create a model object from the model version by calling the [create\_model](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model) method\. Pass the Amazon Resource Name \(ARN\) of the model version as part of the `Containers` for the model object\.

   The following code snippet assumes you have already created the SageMaker Boto3 client `sm_client`, and that you have already created a model version with an ARN that you have stored in a variable named `model_version_arn`\.

   ```
   model_name = 'DEMO-modelregistry-model-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   print("Model name : {}".format(model_name))
   container_list = [{'ModelPackageName': model_version_arn}]
   
   create_model_response = sm_client.create_model(
       ModelName = model_name,
       ExecutionRoleArn = role,
       Containers = container_list
   )
   print("Model arn : {}".format(create_model_response["ModelArn"]))
   ```

1. Create an endpoint configuration by calling `create_endpoint_config`\. The endpoint configuration specifies the number and type of Amazon EC2 instances to use for the endpoint\.

   ```
   endpoint_config_name = 'DEMO-modelregistry-EndpointConfig-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   print(endpoint_config_name)
   create_endpoint_config_response = sm_client.create_endpoint_config(
       EndpointConfigName = endpoint_config_name,
       ProductionVariants=[{
           'InstanceType':'ml.m4.xlarge',
           'InitialVariantWeight':1,
           'InitialInstanceCount':1,
           'ModelName':model_name,
           'VariantName':'AllTraffic'}])
   ```

1. Create the endpoint by calling `create_endpoint`\.

   ```
   endpoint_name = 'DEMO-modelregistry-endpoint-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   print("EndpointName={}".format(endpoint_name))
   
   create_endpoint_response = sm_client.create_endpoint(
       EndpointName=endpoint_name,
       EndpointConfigName=endpoint_config_name)
   print(create_endpoint_response['EndpointArn'])
   ```

## Deploy a Model Version from a Different Account<a name="model-registry-deploy-xaccount"></a>

You can permit an AWS account to deploy model versions that were created in a different account by adding a cross\-account resource policy\. For example, one team in your organization might be responsible for training models, and a different team is responsible for deploying and updating models\. When you create these resource policies, you apply the policy to the specific resource to which you want to grant access\. For more information about cross\-account resource policies in AWS, see [Cross\-account policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic-cross-account.html) in the *AWS Identity and Access Management User Guide*\.

**Note**  
You must use a KMS key to encrypt the [output data config](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_OutputDataConfig.html) action during training for cross\-account model deployment\.

To enable cross\-account model deployment in SageMaker, you have to provide a cross\-account resource policy for the model group that contains the model versions you want to deploy, the Amazon ECR repository where the inference image for the model group resides, and the Amazon S3 bucket where the model versions are stored\.

To be able to deploy a model that was created in a different account, you must have a role that has access to SageMaker actions, such as a role with the `AmazonSageMakerFullAccess` managed policy\. For information about SageMaker managed policies, see [AWS Managed Policies for Amazon SageMaker](security-iam-awsmanpol.md)\.

The following example creates cross\-account policies for all three of these resources, and applies the policies to the resources\.

The example assumes that you previously defined the following variables:
+ `bucket` \- The Amazon S3 bucket where the model versions are stored\.
+ `kms_key_id` \- The KMS key used to encrypt the training output\.
+ `sm_client` \- A SageMaker Boto3 client\.
+ `model_package_group_name` \- The model group to which you want to grant cross\-account access\.
+ `model_package_group_arn` \- The model group arn to which you want to grant cross\-account access\.

```
import json

# The cross-account id to grant access to
cross_account_id = "123456789012"

# Create the policy for access to the ECR repository
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
response = ecr.set_repository_policy(
    registryId = account,
    repositoryName = 'decision-trees-sample',
    policyText = ecr_repository_policy
)

# Create a policy for accessing the S3 bucket
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
response = s3.put_bucket_policy(
    Bucket = bucket,
    Policy = bucket_policy)

# Create the KMS grant for encryption in the source account to the
# model registry account model package group
client = boto3.client('kms')

response = client.create_grant(
    GranteePrincipal=cross_account_id,
    KeyId=kms_key_id
    Operations=[
        'Decrypt',
        'GenerateDataKey',
    ],
)

# 3. Create a policy for access to the model package group.
model_package_group_policy = {
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
model_package_group_policy = json.dumps(model_package_group_policy)

# Set the policy to the model package group
response = sm_client.put_model_package_group_policy(
    ModelPackageGroupName = model_package_group_name,
    ResourcePolicy = model_package_group_policy)

print('ModelPackageGroupArn : {}'.format(create_model_package_group_response['ModelPackageGroupArn']))
print("First Versioned ModelPackageArn: " + model_package_arn)
print("Second Versioned ModelPackageArn: " + model_package_arn2)

print("Success! You are all set to proceed for cross-account deployment.")
```