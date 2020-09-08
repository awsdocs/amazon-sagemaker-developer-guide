# Create a Multi\-Model Endpoint \(AWS SDK for Python \(Boto\)\)<a name="create-multi-model-endpoint-sdk"></a>

You create a multi\-model endpoint using the Amazon SageMaker [ `create_model`](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model), [ `create_endpoint_config`](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_endpoint_config), and [ `create_endpoint`](https://docs.aws.amazon.com/https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_endpoint) APIs just as you would create a single model endpoint, but with two changes\. When defining the model container, you need to pass a new `Mode` parameter value, `MultiModel`\. You also need to pass the `ModelDataUrl` field that specifies the prefix in Amazon S3 where the model artifacts are located, instead of the path to a single model artifact, as you would when deploying a single model\.

For a sample notebook that uses SageMaker to deploy multiple XGBoost models to an endpoint, see [Multi\-Model Endpoint XGBoost Sample Notebook](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/multi_model_xgboost_home_value/xgboost_multi_model_endpoint_home_value.ipynb)\. 

The following procedure outlines the key steps used in that sample to create a multi\-model endpoint\.

**To deploy the model \(AWS SDK for Python \(Boto 3\)\)**

1. Get a container with an image that supports deploying multi\-model endpoints\. For a list of built\-in algorithms and framework containers that support multi\-model endpoints, see [Supported Algorithms and Frameworks](multi-model-endpoints.md#multi-model-support)\. For this example, we use the [K\-Nearest Neighbors \(k\-NN\) Algorithm](k-nearest-neighbors.md) built\-in algorithm\. We call the [SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/v2.html) utility function `image_uris.retrieve()` to get the address for the K\-Nearest Neighbors built\-in algorithm image\.

   ```
   import sagemaker
   image = sagemaker.image_uris.retrieve("knn")
   container = { 'Image':        image,
                 'ModelDataUrl': 's3://my-bucket/path/to/artifacts/',
                 'Mode':         'MultiModel'
               }
   ```

1. Create the model that uses this container\.

   ```
   response = sm_client.create_model(
                 ModelName        = 'my-multi-model-name',
                 ExecutionRoleArn = role,
                 Containers       = [container])
   ```

1. \(Optional\) If you are using a serial inference pipeline, get the additional container\(s\) to include in the pipeline, and include it in the `Containers` argument of `CreateModel`:

   ```
   preprocessor_container = { 'Image':        '123456789012.dkr.ecr.us-east-1.amazonaws.com/mypreprocessorimage:mytag'
               }
   
   multi_model_container = { 'Image':        '763104351884.dkr.ecr.us-east-1.amazonaws.com/myimage:mytag',
                 'ModelDataUrl': 's3://my-bucket/path/to/artifacts/',
                 'Mode':         'MultiModel'
               }
   
   response = sm_client.create_model(
                 ModelName        = 'my-multi-model-name',
                 ExecutionRoleArn = role,
                 Containers       = [preprocessor_container, multi_model_container])
   ```

1. Configure the multi\-model endpoint for the model\. We recommend configuring your endpoints with at least two instances\. This allows SageMaker to provide a highly available set of predictions across multiple Availability Zones for the models\.

   ```
   response = sm_client.create_endpoint_config(
       EndpointConfigName = ‘my-epc’,
       ProductionVariants=[{
           'InstanceType':        'ml.m4.xlarge',
           'InitialInstanceCount': 2,
           'InitialVariantWeight': 1,
           'ModelName':            ‘my-multi-model-name’,
           'VariantName':          'AllTraffic'}])
   ```
**Note**  
You can use only one multi\-model\-enabled endpoint in a serial inference pipeline\.

1. Create the multi\-model endpoint using the `EndpointName` and `EndpointConfigName` parameters\.

   ```
   response = sm_client.create_endpoint(
                 EndpointName       = 'my-endpoint',
                 EndpointConfigName = 'my-epc')
   ```