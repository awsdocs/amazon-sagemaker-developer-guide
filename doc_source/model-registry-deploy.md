# Deploy a Model in the Registry<a name="model-registry-deploy"></a>

After you register a model version and approve it for deployment, deploy it to a SageMaker endpoint for real\-time inference\.

When you create an MLOps project and choose an MLOps project template that includes model deployment, approved model versions in the model registry are automatically deployed to production\. For information about using SageMaker MLOps projects, see [Automate MLOps with SageMaker Projects](sagemaker-projects.md)\.

## Deploy a Model in the Registry \(Boto3\)<a name="model-registry-deploy-api"></a>

To deploy a model version, complete the following steps:

1. Create a model object from the model version by calling `create_model`, passing the Amazon Resource Name \(ARN\) of the model version as the primary container for the model object\. The following code snippet assumes you have already created the SageMaker Boto3 client `sm_client`, and that you have already created a model version with an ARN that you have stored in a variable named `model_version_arn`\.

   ```
   model_name = 'DEMO-modelregistry-model-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   print("Model name : {}".format(model_name))
   primary_container = {
       'ModelPackageName': model_version_arn,
   }
   create_model_respose = sm_client.create_model(
       ModelName = model_name,
       ExecutionRoleArn = role,
       PrimaryContainer = primary_container
   )
   print("Model arn : {}".format(create_model_respose["ModelArn"]))
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
