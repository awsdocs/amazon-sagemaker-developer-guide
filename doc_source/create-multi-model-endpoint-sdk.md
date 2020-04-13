# Create a Multi\-Model Endpoint \(AWS SDK for Python \(Boto\)\)<a name="create-multi-model-endpoint-sdk"></a>

You create a multi\-model endpoint using the Amazon SageMaker [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html), [ `CreateEndpointConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html), and [ `CreateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) APIs just as you would create a single model endpoint, but with two changes\. When defining the container, you need to pass a new `Mode` parameter value, `MultiModel`\. You also need to pass the `ModelDataUrl` field that specifies the prefix in Amazon S3 where the model artifacts are located, instead of the path to a single model artifact, as you would when deploying a single model\.

For a sample notebook that uses Amazon SageMaker to deploy multiple XGBoost models to an endpoint, see [Multi\-Model Endpoint XGBoost Sample Notebook](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/multi_model_xgboost_home_value/xgboost_multi_model_endpoint_home_value.ipynb)\. 

The following procedure outlines the key steps used in that sample to create a multi\-model endpoint\.

**To deploy the model \(AWS SDK for Python \(Boto 3\)\)**

1. Get a container whose image supports deploying models\.

   ```
   container = { 'Image':        '123456789012.dkr.ecr.us-east-1.amazonaws.com/myimage:mytag',
                 'ModelDataUrl': ‘s3://my-bucket/path/to/artifacts/’,
                 'Mode':         'MultiModel'
               }
   ```

1. Create the model that uses this container\.

   ```
   response = sm_client.create_model(
                 ModelName        = ‘my-multi-model-name’,
                 ExecutionRoleArn = role,
                 Containers       = [container])
   ```

1. Configure the multi\-model endpoint for the model\. We recommend configuring your endpoints with at least two instances\. This allows Amazon SageMaker to provide a highly available set of predictions across multiple Availability Zones for the models\.

   ```
   response = sm_client.create_endpoint_config(
       EndpointConfigName = ‘my-epc’,
       ProductionVariants=[{
           'InstanceType':        ‘ml.m4.xlarge’,
           'InitialInstanceCount': 2,
           'InitialVariantWeight': 1,
           'ModelName':            ‘my-multi-model-name’,
           'VariantName':          'AllTraffic'}])
   ```

1. Create the multi\-model endpoint using the `EndpointName` and `EndpointConfigName` parameters\.

   ```
   response = sm_client.create_endpoint(
                 EndpointName       = ‘my-endpoint’,
                 EndpointConfigName = ‘my-epc’)
   ```