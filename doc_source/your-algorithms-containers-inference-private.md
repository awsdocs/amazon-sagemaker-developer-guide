# Use a Private Docker Registry for Real\-Time Inference Containers<a name="your-algorithms-containers-inference-private"></a>

Amazon SageMaker hosting enables you to use images stored in Amazon ECR to build your containers for real\-time inference by default\. Optionally, you can build containers for real\-time inference from images in a private Docker registry\. The private registry must be accessible from an Amazon VPC in your account\. Models that you create based on the images stored in your private Docker registry must be configured to connect to the same VPC where the private Docker registry is accessible\. For information about connecting your model to a VPC, see [Give SageMaker Hosted Endpoints Access to Resources in Your Amazon VPC](host-vpc.md)\.

Your Docker registry must be secured with a TLS certificate from a known public certificate authority \(CA\)\.

**Note**  
Your private Docker registry must allow inbound traffic from the security groups you specify in the VPC configuration for your model, so that SageMaker hosting is able to pull model images from your registry\.  
SageMaker can pull model images from DockerHub if there's a path to the open internet inside your VPC\.

**Topics**
+ [Store Images in a Private Docker Registry other than Amazon Elastic Container Registry](#your-algorithms-containers-inference-private-registry)
+ [Use an Image from a Private Docker Registry for Real\-time Inference](#your-algorithms-containers-inference-private-use)
+ [Allow SageMaker to authenticate to a private Docker registry](#inference-private-docker-authenticate)
+ [Create the Lambda function](#inference-private-docker-lambda)
+ [Give your execution role permission to Lambda](#inference-private-docker-perms)
+ [Create an interface VPC endpoint for Lambda](#inference-private-docker-vpc-interface)

## Store Images in a Private Docker Registry other than Amazon Elastic Container Registry<a name="your-algorithms-containers-inference-private-registry"></a>

To use a private Docker registry to store your images for SageMaker real\-time inference, create a private registry that is accessible from your Amazon VPC\. For information about creating a Docker registry, see [Deploy a registry server](https://docs.docker.com/registry/deploying/) in the Docker documentation\. The Docker registry must comply with the following:
+ The registry must be a [Docker Registry HTTP API V2](https://docs.docker.com/registry/spec/api/) registry\.
+ The Docker registry must be accessible from the same VPC that you specify in the `VpcConfig` parameter that you specify when you create your model\.

## Use an Image from a Private Docker Registry for Real\-time Inference<a name="your-algorithms-containers-inference-private-use"></a>

When you create a model and deploy it to SageMaker hosting, you can specify that it use an image from your private Docker registry to build the inference container\. Specify this in the `ImageConfig` object in the `PrimaryContainer` parameter that you pass to a call to the [create\_model](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model) function\.

**To use an image stored in your private Docker registry for your inference container**

1. Create the image configuration object and specify a value of `Vpc` for the `RepositoryAccessMode` field\.

   ```
   image_config = {
                       'RepositoryAccessMode': 'Vpc'
                  }
   ```

1. If your private Docker registry requires authentication, add a `RepositoryAuthConfig` object to the image configuration object\. For the `RepositoryCredentialsProviderArn` field of the `RepositoryAuthConfig` object, specify the Amazon Resource Name \(ARN\) of an AWS Lambda function that provides credentials that allows SageMaker to authenticate to your private Docker Registry\. For information about how to create the Lambda function to provide authentication, see [Allow SageMaker to authenticate to a private Docker registry](#inference-private-docker-authenticate)\.

   ```
   image_config = {
                       'RepositoryAccessMode': 'Vpc',
                       'RepositoryAuthConfig': {
                          'RepositoryCredentialsProviderArn': 'arn:aws:lambda:Region:Acct:function:FunctionName'
                        }
                  }
   ```

1. Create the primary container object that you want to pass to `create_model`, using the image configuration object that you created in the previous step\.

   ```
   primary_container = {
       'ContainerHostname': 'ModelContainer',
       'Image': 'myteam.myorg.com/docker-local/my-inference-image:latest',
       'ImageConfig': image_config
   }
   ```

1. Specify the model name and the execution role that you want to pass to `create_model`\.

   ```
   model_name = 'vpc-model'
   execution_role_arn = 'arn:aws:iam::123456789012:role/SageMakerExecutionRole'
   ```

1. Specify one or more security groups and subnets for the VPC configuration for your model\. Your private Docker registry must allow inbound traffic from the security groups that you specify\. The subnets that you specify must be in the same VPC as your private Docker registry\.

   ```
   vpc_config = {
       'SecurityGroupIds': ['sg-0123456789abcdef0'],
       'Subnets': ['subnet-0123456789abcdef0','subnet-0123456789abcdef1']
   }
   ```

1. Get a Boto3 SageMaker client\.

   ```
   import boto3
   sm = boto3.client('sagemaker')
   ```

1. Create the model by calling `create_model`, using the values you specified in the previous steps for the `PrimaryContainer` and `VpcConfig` parameters\.

   ```
   try:
       resp = sm.create_model(
           ModelName=model_name,
           PrimaryContainer=primary_container,
           ExecutionRoleArn=execution_role_arn,
           VpcConfig=vpc_config,
       )
   except Exception as e:
       print(f'error calling CreateModel operation: {e}')
   else:
       print(resp)
   ```

1. Finally, call [create\_endpoint\_config](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_endpoint_config) and [create\_endpoint](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_endpoint) to create the hosting endpoint, using the model that you created in the previous step\.

   ```
   endpoint_config_name = 'my-endpoint-config'
   sm.create_endpoint_config(
       EndpointConfigName=endpoint_config_name,
       ProductionVariants=[
           {
               'VariantName': 'MyVariant',
               'ModelName': model_name,
               'InitialInstanceCount': 1,
               'InstanceType': 'ml.t2.medium'
           },
       ],
   )
   
   endpoint_name = 'my-endpoint'
   sm.create_endpoint(
       EndpointName=endpoint_name,
       EndpointConfigName=endpoint_config_name,
   )
   
   sm.describe_endpoint(EndpointName=endpoint_name)
   ```

## Allow SageMaker to authenticate to a private Docker registry<a name="inference-private-docker-authenticate"></a>

To pull an inference image from a private Docker registry that requires authentication, create an AWS Lambda function that provides credentials, and provide the Amazon Resource Name \(ARN\) of the Lambda function when you call [create\_model](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model)\. When SageMaker runs `create_model`, it calls the Lambda function that you specified to get credentials to authenticate to your Docker registry\.

## Create the Lambda function<a name="inference-private-docker-lambda"></a>

Create an AWS Lambda function that returns a response with the following form:

```
def handler(event, context):
   response = {
      "Credentials": {"Username": "username", "Password": "password"}
   }
   return response
```

Depending on how you set up authentication for your private Docker registry, the credentials that your Lambda function returns can mean either of the following:
+ If you set up your private Docker registry to use basic authentication, this is the *username* and *password* to authenticate to the registry\.
+ If you set up your private Docker registry to use bearer token authentication, the *username* and *password* are sent to your authorization server, which returns a Bearer token that can then be used to authenticate to the private Docker registry\.

## Give your execution role permission to Lambda<a name="inference-private-docker-perms"></a>

The execution role that you use to call `create_model` must have permissions to call AWS Lambda functions\. Add the following to the permissions policy of your execution role\.

```
{
    "Effect": "Allow",
    "Action": [
        "lambda:InvokeFunction"
    ],
    "Resource": [
        "arn:aws:lambda:*:*:function:*myLambdaFunction*"
    ]
}
```

Where *myLambdaFunction* is the name of your Lambda function\. For information about editing a role permissions policy, see [Modifying a role permissions policy \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_permissions-policy) in the *AWS Identity and Access Management User Guide*\.

**Note**  
An execution role with the `AmazonSageMakerFullAccess` managed policy attached to it has permission to call any Lambda function with **SageMaker** in its name\.

## Create an interface VPC endpoint for Lambda<a name="inference-private-docker-vpc-interface"></a>

Create an interface endpoint so that your Amazon VPC can communicate with your AWS Lambda function without sending traffic over the internet\. For information about how to do this, see [Configuring interface VPC endpoints for Lambda](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc-endpoints.html) in the *AWS Lambda Developer Guide*\.

SageMaker hosting sends a request through your VPC to `lambda.region.amazonaws.com`, to call your Lambda function\. If you choose Private DNS Name when you create your interface endpoint, Amazon Route 53 routes the call to the Lambda interface endpoint\. If you use a different DNS provider, make sure to map `lambda.region.amazonaws.com` to your Lambda interface endpoint\.