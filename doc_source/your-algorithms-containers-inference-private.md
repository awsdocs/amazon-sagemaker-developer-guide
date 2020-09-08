# Use a Private Docker Registry for Real\-Time Inference Containers<a name="your-algorithms-containers-inference-private"></a>

Amazon SageMaker hosting enables you to use images stored in Amazon ECR to build your containers for real\-time inference by default\. Optionally, you can build containers for real\-time inference from images in a private Docker registry\. The private registry must be accessible from an Amazon VPC in your account\. Models that you create based on the images stored in your private Docker registry must be configured to connect to the same VPC where you create the private Docker registry\. For information about connecting your model to a VPC, see [Give SageMaker Hosted Endpoints Access to Resources in Your Amazon VPC](host-vpc.md)\.

Your Docker registry must be secured with a TLS certificate from a known public certificate authority \(CA\)\.

**Note**  
Your private Docker registry must allow inbound traffic from the security group\(s\) you specify in the VPC configuration for your model, so that SageMaker hosting is able to pull model images from your registry\.

**Topics**
+ [Store Images in a Private Docker Registry](#your-algorithms-containers-inference-private-registry)
+ [Use an Image from a Private Docker Registry for Real\-time Inference](#your-algorithms-containers-inference-private-use)

## Store Images in a Private Docker Registry<a name="your-algorithms-containers-inference-private-registry"></a>

To use a private Docker registry to store your images for SageMaker real\-time inference, create a private registry that is accessible from your Amazon VPC\. For information about creating a Docker registry, see [Deploy a registry server](https://docs.docker.com/registry/deploying/) in the Docker documentation\. The Docker registry must comply with the following:
+ The registry must be a [Docker Registry HTTP API V2](https://docs.docker.com/registry/spec/api/) registry\.
+ The Docker registry must be accessible from the same VPC that you specify in the `VpcConfig` parameter that you specify when you create your model\.
+ The Docker registry must not require any authentication\.

## Use an Image from a Private Docker Registry for Real\-time Inference<a name="your-algorithms-containers-inference-private-use"></a>

When you create a model and deploy it to SageMaker hosting, you can specify that it use an image from your private Docker registry to build the inference container\.

**To use an image stored in your private Docker registry for your inference container**

1. Specify `Vpc` as the value of the `RepositoryAccess` field of the `ImageConfig` setting for the primary container when you call the [create\_model](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model) function\.

   ```
   model_name = 'vpc-model'
   execution_role_arn = 'arn:aws:iam::123456789012:role/SageMakerExecutionRole'
   primary_container = {
       'ContainerHostname': 'ModelContainer',
       'Image': 'myteam.myorg.com/docker-local/my-inference-image:latest',
       'ImageConfig': {
           'RepositoryAccessMode': 'Vpc',
       },
   }
   ```

1. Specify one or more security groups and subnets for the VPC configuration for your model\. Your private Docker registry must allow inbound traffic from the security group\(s\) that you specify\. The subnets that you specify must be in the same VPC as your private Docker registry\.

   ```
   vpc_config = {
       'SecurityGroupIds': ['sg-0123456789abcdef0'],
       'Subnets': ['subnet-0123456789abcdef0','subnet-0123456789abcdef1']
   }
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