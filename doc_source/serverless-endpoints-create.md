# Create a serverless endpoint<a name="serverless-endpoints-create"></a>


****  

|  | 
| --- |
| Serverless Inference is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

To create a serverless endpoint, you can use the Amazon SageMaker console, the APIs, or the AWS CLI\. You can create a serverless endpoint using a similar process as a [real\-time endpoint](realtime-endpoints.md)\.

**Topics**
+ [Create a model](#serverless-endpoints-create-model)
+ [Create an endpoint configuration](#serverless-endpoints-create-config)
+ [Create an endpoint](#serverless-endpoints-create-endpoint)

## Create a model<a name="serverless-endpoints-create-model"></a>

To create your model, you must provide the location of your model artifacts and container image\. The examples in the following sections show you how to create a model using the [CreateModel](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) API and the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home)\.

### To create a model \(API\)<a name="serverless-endpoints-create-model-api"></a>

The following example uses the [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#id309) to call the [CreateModel](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) API\. Specify the following values:
+ For `sagemaker_role,` you can use the default SageMaker\-created role or a customized SageMaker IAM role from Step 4 of the [Prerequisites](serverless-endpoints-prerequisites.md) section\.
+ For `model_url`, specify the Amazon S3 URI to your model\.
+ For `container`, retrieve the container you want to use by its Amazon ECR path\. This example uses a SageMaker\-provided XGBoost container\. If you have not selected a SageMaker container or brought your own, see Step 6 of the [Prerequisites](serverless-endpoints-prerequisites.md) section for more information\.
+ For `model_name`, enter a name for the model\.

```
#Setup
import boto3
import sagemaker
region = boto3.Session().region_name
client = boto3.client("sagemaker", region_name=region)

#Role to give SageMaker permission to access AWS services.
sagemaker_role = sagemaker.get_execution_role()

#Get model from S3
model_url = "s3://DOC-EXAMPLE-BUCKET/models/model.tar.gz"

#Get container image (prebuilt example)
from sagemaker import image_uris
container = image_uris.retrieve("xgboost", region, "0.90-1")

#Create model
model_name = "<name-for-model>"

response = client.create_model(
    ModelName = model_name,
    ExecutionRoleArn = sagemaker_role,
    Containers = [{
        "Image": container,
        "Mode": "SingleModel",
        "ModelDataUrl": model_url,
    }]
)
```

### To create a model \(console\)<a name="serverless-endpoints-create-model-console"></a>

1. Sign in to the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home)\.

1. In the navigation tab, choose **Inference**\.

1. Next, choose **Models**\.

1. Choose **Create model**\.

1. For **Model name**, enter a name for the model that is unique to your account and AWS Region\.

1. For **IAM role**, either select an IAM role you have already created \(see [Prerequisites](serverless-endpoints-prerequisites.md)\) or allow SageMaker to create one for you\.

1. In **Container definition 1**, for **Container input options**, select **Provide model artifacts and input location**\.

1. For **Provide model artifacts and inference image options**, select **Use a single model**\.

1. For **Location of inference code image**, enter an Amazon ECR path to a container\. The image must either be a SageMaker\-provided first party image \(e\.g\. TensorFlow, XGBoost\) or an image that resides in an Amazon ECR repository within the same account in which you are creating the endpoint\. If you do not have a container, go back to Step 6 of the [Prerequisites](serverless-endpoints-prerequisites.md) section for more information\.

1. For **Location of model artifacts**, enter the Amazon S3 URI to your ML model\. For example, `s3://DOC-EXAMPLE-BUCKET/models/model.tar.gz`\.

1. \(Optional\) For **Tags**, add key\-value pairs to create metadata for your model\.

1. Choose **Create model**\.

## Create an endpoint configuration<a name="serverless-endpoints-create-config"></a>

After you create a model, create an endpoint configuration\. You can then deploy your model using the specifications in your endpoint configuration\. In the configuration, you specify whether you want a real\-time or serverless endpoint\. To create a serverless endpoint configuration, you can use the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home), the [CreateEndpointConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) API, or the AWS CLI\. The API and console approaches are outlined in the following sections\.

### To create an endpoint configuration \(API\)<a name="serverless-endpoints-create-config-api"></a>

The following example uses the [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#id309) to call the [CreateEndpointConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) API\. Specify the following values:
+ For `EndpointConfigName`, choose a name for the endpoint configuration\. The name should be unique within your account in a Region\.
+ For `ModelName`, use the name of the model you want to deploy\. It should be the same model that you used in the [Create a model](#serverless-endpoints-create-model) step\.
+ For `ServerlessConfig`:
  + Set `MemorySizeInMB` to `2048`\. For this example, we set the memory size to 2048 MB, but you can choose any of the following values for your memory size: 1024 MB, 2048 MB, 3072 MB, 4096 MB, 5120 MB, or 6144 MB\. 
  + Set `MaxConcurrency` to `20`\. For this example, we set the maximum concurrency to 20\. The maximum number of concurrent invocations you can set for a serverless endpoint is 50, and the minimum value you can choose is 1\.

```
response = client.create_endpoint_config(
   EndpointConfigName="<your-endpoint-configuration>",
   ProductionVariants=[
        {
            "ModelName": "<your-model-name>",
            "VariantName": "AllTraffic",
            "ServerlessConfig": {
                "MemorySizeInMB": 2048,
                "MaxConcurrency": 20
            }
        } 
    ]
)
```

### To create an endpoint configuration \(console\)<a name="serverless-endpoints-create-config-console"></a>

1. Sign in to the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home)\.

1. In the navigation tab, choose **Inference**\.

1. Next, choose **Endpoint configurations**\.

1. Choose **Create endpoint configuration**\.

1. For **Endpoint configuration name**, enter a name that is unique within your account in a Region\.

1. For **Type of endpoint**, select **Serverless \(In Preview\)**\.  
![\[Screenshot of the endpoint type option in the console.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/serverless-endpoints-endpoint-config.png)

1. For **Production variants**, choose **Add model**\.

1. Under **Add model**, select the model you want to use from the list of models and then choose **Save**\.

1. After adding your model, under **Actions**, choose **Edit**\.

1. For **Memory size**, choose the memory size you want in GB\.  
![\[Screenshot of the memory size option in the console.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/serverless-endpoints-endpoint-config-2.png)

1. For **Max Concurrency**, enter your desired maximum concurrent invocations for the endpoint\. The maximum value you can enter is 50 and the minimum is 1\.

1. Choose **Save**\.

1. \(Optional\) For **Tags**, enter key\-value pairs if you want to create metadata for your endpoint configuration\.

1. Choose **Create endpoint configuration**\.

## Create an endpoint<a name="serverless-endpoints-create-endpoint"></a>

To create a serverless endpoint, you can use the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home), the [CreateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API, or the AWS CLI\. The API and console approaches are outlined in the following sections\. Once you create your endpoint, it can take several minutes for the endpoint to become available\.

### To create an endpoint \(API\)<a name="serverless-endpoints-create-endpoint-api"></a>

The following example uses the [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#id309) to call the [CreateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API\. Specify the following values:
+ For `EndpointName`, enter a name for the endpoint that is unique within a Region in your account\.
+ For `EndpointConfigName`, use the name of the endpoint configuration that you created in the previous section\.

```
response = client.create_endpoint(
    EndpointName="<your-endpoint-name>",
    EndpointConfigName="<your-endpoint-config>"
)
```

### To create an endpoint \(console\)<a name="serverless-endpoints-create-endpoint-console"></a>

1. Sign in to the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home)\.

1. In the navigation tab, choose **Inference**\.

1. Next, choose **Endpoints**\.

1. Choose **Create endpoint**\.

1. For **Endpoint name**, enter a name than is unique within a Region in your account\.

1. For **Attach endpoint configuration**, select **Use an existing endpoint configuration**\.

1. For **Endpoint configuration**, select the name of the endpoint configuration you created in the previous section and then choose **Select endpoint configuration**\.

1. \(Optional\) For **Tags**, enter key\-value pairs if you want to create metadata for your endpoint\.

1. Choose **Create endpoint**\.  
![\[Screenshot of the create and configure endpoint page in the console.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/serverless-endpoints-create.png)