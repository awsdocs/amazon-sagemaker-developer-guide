# Create a Multi\-Model Endpoint<a name="create-multi-model-endpoint"></a>

You can use AWS SDK for Python \(Boto\) or Amazon SageMaker to create a multi\-model endpoint\.

**Topics**
+ [Create a Multi\-Model Endpoint \(Console\)](#create-multi-model-endpoint-console)
+ [Create a Multi\-Model Endpoint AWS SDK for Python \(Boto3\)](#create-multi-model-endpoint-sdk)

## Create a Multi\-Model Endpoint \(Console\)<a name="create-multi-model-endpoint-console"></a>



**To create a multi\-model endpoint \(console\)**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Model**, and then from the **Inference** group, choose **Create model**\. 

1. For **Model name**, enter a name\.

1. For **IAM role**\. choose or create an IAM role that has the AmazonSageMakerFullAccess IAM policy attached\. 

1.  In the **Container definition** section, for **Provide model artifacts and inference image options**choose **Use multiple models**\.  
![\[The section of the Create model page where you can choose Use multiple models to host multiple models on a single endpoint.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/mme-create-model-ux-2.PNG)

1. Choose **Create model**\.

1. Deploy your multi\-model endpoint as you would a single model endpoint\. For instructions, see [Deploy the Model to SageMaker Hosting Services](ex1-model-deployment.md#ex1-deploy-model)\.

## Create a Multi\-Model Endpoint AWS SDK for Python \(Boto3\)<a name="create-multi-model-endpoint-sdk"></a>

You create a multi\-model endpoint using the Amazon SageMaker [ `create_model`](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model), [ `create_endpoint_config`](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_endpoint_config), and [ `create_endpoint`](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_endpoint) APIs just as you would create a single model endpoint, but with two changes\. When defining the model container, you need to pass a new `Mode` parameter value, `MultiModel`\. You also need to pass the `ModelDataUrl` field that specifies the prefix in Amazon S3 where the model artifacts are located, instead of the path to a single model artifact, as you would when deploying a single model\.

For a sample notebook that uses SageMaker to deploy multiple XGBoost models to an endpoint, see [Multi\-Model Endpoint XGBoost Sample Notebook](https://sagemaker-examples.readthedocs.io/en/latest/advanced_functionality/multi_model_xgboost_home_value/xgboost_multi_model_endpoint_home_value.html)\. 

The following procedure outlines the key steps used in that sample to create a multi\-model endpoint\.

**To deploy the model \(AWS SDK for Python \(Boto 3\)\)**

1. Get a container with an image that supports deploying multi\-model endpoints\. For a list of built\-in algorithms and framework containers that support multi\-model endpoints, see [Supported Algorithms and Frameworks](multi-model-endpoints.md#multi-model-support)\. For this example, we use the [K\-Nearest Neighbors \(k\-NN\) Algorithm](k-nearest-neighbors.md) built\-in algorithm\. We call the [SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/v2.html) utility function `image_uris.retrieve()` to get the address for the K\-Nearest Neighbors built\-in algorithm image\.

   ```
   import sagemaker
   region = sagemaker_session.boto_region_name
   image = sagemaker.image_uris.retrieve("knn",region=region)
   container = { 
                 'Image':        image,
                 'ModelDataUrl': 's3://<BUCKET_NAME>/<PATH_TO_ARTIFACTS>',
                 'Mode':         'MultiModel'
               }
   ```

1. Get a Boto 3 SageMaker client and create the model that uses this container\.

   ```
   import boto3
   sagemaker_client = boto3.client('sagemaker')
   response = sagemaker_client.create_model(
                 ModelName        = '<MODEL_NAME>',
                 ExecutionRoleArn = role,
                 Containers       = [container])
   ```

1. \(Optional\) If you are using a serial inference pipeline, get the additional container\(s\) to include in the pipeline, and include it in the `Containers` argument of `CreateModel`:

   ```
   preprocessor_container = { 
                  'Image': '<ACCOUNT_ID>.dkr.ecr.<REGION_NAME>.amazonaws.com/<PREPROCESSOR_IMAGE>:<TAG>'
               }
   
   multi_model_container = { 
                 'Image': '<ACCOUNT_ID>.dkr.ecr.<REGION_NAME>.amazonaws.com/<IMAGE>:<TAG>',
                 'ModelDataUrl': 's3://<BUCKET_NAME>/<PATH_TO_ARTIFACTS>',
                 'Mode':         'MultiModel'
               }
   
   response = sagemaker_client.create_model(
                 ModelName        = '<MODEL_NAME>',
                 ExecutionRoleArn = role,
                 Containers       = [preprocessor_container, multi_model_container]
               )
   ```

1. \(Optional\) If your use case does not benefit from model caching, set the value of the `ModelCacheSetting` field of the `MultiModelConfig` parameter to `Disabled`, and include it in the `Container` argument of the call to `create_model`\. The value of the `ModelCacheSetting` field is `Enabled` by default\.

   ```
   container = { 
                   'Image': image, 
                   'ModelDataUrl': 's3://<BUCKET_NAME>/<PATH_TO_ARTIFACTS>',
                   'Mode': 'MultiModel' 
                   'MultiModelConfig': {
                           // Default value is 'Enabled'
                           'ModelCacheSetting': 'Disabled'
                   }
              }
   
   response = sagemaker_client.create_model(
                 ModelName        = '<MODEL_NAME>',
                 ExecutionRoleArn = role,
                 Containers       = [container]
               )
   ```

1. Configure the multi\-model endpoint for the model\. We recommend configuring your endpoints with at least two instances\. This allows SageMaker to provide a highly available set of predictions across multiple Availability Zones for the models\.

   ```
   response = sagemaker_client.create_endpoint_config(
                   EndpointConfigName = '<ENDPOINT_CONFIG_NAME>',
                   ProductionVariants=[
                        {
                           'InstanceType':        'ml.m4.xlarge',
                           'InitialInstanceCount': 2,
                           'InitialVariantWeight': 1,
                           'ModelName':            '<MODEL_NAME>',
                           'VariantName':          'AllTraffic'
                         }
                   ]
              )
   ```
**Note**  
You can use only one multi\-model\-enabled endpoint in a serial inference pipeline\.

1. Create the multi\-model endpoint using the `EndpointName` and `EndpointConfigName` parameters\.

   ```
   response = sagemaker_client.create_endpoint(
                 EndpointName       = '<ENDPOINT_NAME>',
                 EndpointConfigName = '<ENDPOINT_CONFIG_NAME>')
   ```