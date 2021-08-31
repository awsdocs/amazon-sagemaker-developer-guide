# Deploy a Model in the Registry<a name="model-registry-deploy"></a>

After you register a model version and approve it for deployment, deploy it to a SageMaker endpoint for real\-time inference\.

When you create an MLOps project and choose an MLOps project template that includes model deployment, approved model versions in the model registry are automatically deployed to production\. For information about using SageMaker MLOps projects, see [Automate MLOps with SageMaker Projects](sagemaker-projects.md)\.

## Deploy a Model in the Registry \(SageMaker SDK\)<a name="model-registry-deploy-smsdk"></a>

To deploy a model version using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io):

```
from sagemaker import ModelPackage

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
   container_list = [{ModelPackageName: model_version_arn}]
   
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