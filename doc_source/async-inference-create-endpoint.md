# Create an Asynchronous Inference Endpoint<a name="async-inference-create-endpoint"></a>

Create an asynchronous endpoint the same way you would create an endpoint using SageMaker hosting services:
+ Create a model in SageMaker with `CreateModel`\.
+ Create an endpoint configuration with `CreateEndpointConfig`\.
+ Create an HTTPS endpoint with `CreateEndpoint`\.

To create an endpoint, you first create a model with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html), where you point to the model artifact and a Docker registry path \(Image\)\. You then create a configuration using [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) where you specify one or more models that were created using the `CreateModel` API to deploy and the resources that you want SageMaker to provision\. Create your endpoint with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) using the endpoint configuration specified in the request\. You can update an asynchronous endpoint with the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API\. Send and receive inference requests from the model hosted at the endpoint with `InvokeEndpointAsync`\. You can delete your endpoints with the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpoint.html) API\.

For a full list of the available SageMaker Images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. See [Use Your Own Inference Code](your-algorithms-inference-main.md) for information on how to create your Docker image\.

## Create a Model<a name="async-inference-create-endpoint-create-model"></a>

The following example shows how to create a model using the AWS SDK for Python \(Boto3\)\. The first few lines define:
+ `sagemaker_client`: A low\-level SageMaker client object that makes it easy to send and receive requests to AWS services\.
+ `sagemaker_role`: A string variable with the SageMaker IAM role Amazon Resource Name \(ARN\)\.
+ `aws_region`: A string variable with the name of your AWS region\.

```
import boto3

# Specify your AWS Region
aws_region='<aws_region>'

# Create a low-level SageMaker service client.
sagemaker_client = boto3.client('sagemaker', region_name=aws_region)

# Role to give SageMaker permission to access AWS services.
sagemaker_role= "arn:aws:iam::<account>:role/*"
```

Next, specify the location of the pre\-trained model stored in Amazon S3\. In this example, we use a pre\-trained XGBoost model named `demo-xgboost-model.tar.gz`\. The full Amazon S3 URI is stored in a string variable `model_url`:

```
#Create a variable w/ the model S3 URI
s3_bucket = '<your-bucket-name>' # Provide the name of your S3 bucket
bucket_prefix='saved_models'
model_s3_key = f"{bucket_prefix}/demo-xgboost-model.tar.gz"

#Specify S3 bucket w/ model
model_url = f"s3://{s3_bucket}/{model_s3_key}"
```

Specify a primary container\. For the primary container, you specify the Docker image that contains inference code, artifacts \(from prior training\), and a custom environment map that the inference code uses when you deploy the model for predictions\.

 In this example, we specify an XGBoost built\-in algorithm container image: 

```
from sagemaker import image_uris

# Specify an AWS container image. 
container = image_uris.retrieve(region=aws_region, framework='xgboost', version='0.90-1')
```

Create a model in Amazon SageMaker with `CreateModel`\. Specify the following:
+ `ModelName`: A name for your model \(in this example it is stored as a string variable called `model_name`\)\.
+ `ExecutionRoleArn`: The Amazon Resource Name \(ARN\) of the IAM role that Amazon SageMaker can assume to access model artifacts and Docker images for deployment on ML compute instances or for batch transform jobs\.
+ `PrimaryContainer`: The location of the primary Docker image containing inference code, associated artifacts, and custom environment maps that the inference code uses when the model is deployed for predictions\.

```
model_name = '<The_name_of_the_model>'

#Create model
create_model_response = sagemaker_client.create_model(
    ModelName = model_name,
    ExecutionRoleArn = sagemaker_role,
    PrimaryContainer = {
        'Image': container,
        'ModelDataUrl': model_url,
    })
```

See [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) description in the SageMaker API Reference Guide for a full list of API parameters\.

## Create an Endpoint Configuration<a name="async-inference-create-endpoint-create-endpoint-config"></a>

Once you have a model, create an endpoint configuration with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)\. Amazon SageMaker hosting services uses this configuration to deploy models\. In the configuration, you identify one or more models, created using with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html), to deploy the resources that you want Amazon SageMaker to provision\. Specify the `AsyncInferenceConfig` object and provide an output Amazon S3 location for `OutputConfig`\. You can optionally specify [Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) topics on which to send notifications about prediction results\. For more information about Amazon SNS topics, see [Configuring Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-configuring.html)\.

The following example shows how to create an endpoint configuration using AWS SDK for Python \(Boto3\):

```
import datetime
from time import gmtime, strftime

# Create an endpoint config name. Here we create one based on the date  
# so it we can search endpoints based on creation time.
endpoint_config_name = f"XGBoostEndpointConfig-{strftime('%Y-%m-%d-%H-%M-%S', gmtime())}"

# The name of the model that you want to host. This is the name that you specified when creating the model.
model_name='<The_name_of_your_model>'

create_endpoint_config_response = sagemaker_client.create_endpoint_config(
    EndpointConfigName=endpoint_config_name, # You will specify this name in a CreateEndpoint request.
    # List of ProductionVariant objects, one for each model that you want to host at this endpoint.
    ProductionVariants=[
        {
            "VariantName": "variant1", # The name of the production variant.
            "ModelName": model_name, 
            "InstanceType": "ml.m5.xlarge", # Specify the compute instance type.
            "InitialInstanceCount": 1 # Number of instances to launch initially.
        }
    ],
    AsyncInferenceConfig={
        "OutputConfig": {
            # Location to upload response outputs when no location is provided in the request.
            "S3OutputPath": f"s3://{s3_bucket}/{bucket_prefix}/output"
            # (Optional) specify Amazon SNS topics
            "NotificationConfig": {
                "SuccessTopic": "arn:aws:sns:aws-region:account-id:topic-name",
                "ErrorTopic": "arn:aws:sns:aws-region:account-id:topic-name",
            }
        },
        "ClientConfig": {
            # (Optional) Specify the max number of inflight invocations per instance
            # If no value is provided, Amazon SageMaker will choose an optimal value for you
            "MaxConcurrentInvocationsPerInstance": 4
        }
    }
)

print(f"Created EndpointConfig: {create_endpoint_config_response['EndpointConfigArn']}")
```

In the aforementioned example, you specify the following keys for `OutputConfig` for the `AsyncInferenceConfig` field:
+ `S3OutputPath`: Location to upload response outputs when no location is provided in the request\.
+ `NotificationConfig`: \(Optional\) SNS topics that post notifications to you when an inference request is successful \(`SuccessTopic`\) or if it fails \(`ErrorTopic`\)\.

You can also specify the following optional argument for `ClientConfig` in the `AsyncInferenceConfig` field:
+ `MaxConcurrentInvocationsPerInstance`: \(Optional\) The maximum number of concurrent requests sent by the SageMaker client to the model container\.

## Create Endpoint<a name="async-inference-create-endpoint-create-endpoint"></a>

Once you have your model and endpoint configuration, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API to create your endpoint\. The endpoint name must be unique within an AWS Region in your AWS account\. 

The following creates an endpoint using the endpoint configuration specified in the request\. Amazon SageMaker uses the endpoint to provision resources and deploy models\.

```
# The name of the endpoint.The name must be unique within an AWS Region in your AWS account.
endpoint_name = '<endpoint-name>' 

# The name of the endpoint configuration associated with this endpoint.
endpoint_config_name='<endpoint-config-name>'

create_endpoint_response = sagemaker_client.create_endpoint(
                                            EndpointName=endpoint_name, 
                                            EndpointConfigName=endpoint_config_name)
```

When you call the `CreateEndpoint` API, Amazon SageMaker Asynchronous Inference sends a test notification to check that you have configured an Amazon SNS topic\. This lets SageMaker check that you have the required permissions\. The notification can simply be ignored\. The test notification has the following form:

```
{
    "eventVersion":"1.0",
    "eventSource":"aws:sagemaker",
    "eventName":"TestNotification"
}
```