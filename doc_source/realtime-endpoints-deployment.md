# Create your endpoint and deploy your model<a name="realtime-endpoints-deployment"></a>

There are several options to deploy a model using SageMaker hosting services\. You can programmatically deploy a model using an AWS SDK \(for example, the SDK for Python \(Boto3\)\), the SageMaker Python SDK, the AWS CLI, or you can interactively deploy a model with the SageMaker console\. The AWS SDK is a low\-level API and supports Java, C\+\+, Go, JavaScript, Node\.js, PHP, Ruby, and Python whereas the SageMaker Python SDK is a high\-level Python API\. The following documentation demonstrates how to deploy a model using the AWS SDK for Python \(Boto3\) and the SageMaker Python SDK\.

 Deploying a model using SageMaker hosting services is a three\-step process if you use AWS SDK for Python \(Boto3\), the AWS CLI, or the SageMaker console:

1. Create a SageMaker model in SageMaker\.

1. Create an endpoint configuration for an HTTPS endpoint\.

1. Create an HTTPS endpoint\.

Deploying a model using the SageMaker Python SDK does not require that you create an endpoint configuration\. It is therefore a two\-step process:

1. Create a model object from the `Model` Class that can be deployed to an HTTPS endpoint\.

1. Create an HTTPS endpoint with the Model object's pre\-built `deploy()` method\.

To use the proceeding code snippets, replace the *italicized placeholder text* in the example code with your own information\.

**Note**  
 SageMaker supports running \(a\) multiple models, \(b\) multiple variants of a model, or \(c\) combinations of models and variants on the same endpoint\. Model variants can reference the same model inference container \(i\.e\. run the same algorithm\), but use different model artifacts \(e\.g\., different model weight values based on other hyper\-parameter configurations\)\. In contrast, two different models may use the same algorithm, but focus on different business problems or underlying goals and may operate on different data sets\. 
When you create an endpoint, SageMaker attaches an Amazon EBS storage volume to each ML compute instance that hosts the endpoint\. The size of the storage volume depends on the instance type\. For a list of instance types that SageMaker hosting service supports, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_sagemaker)\. For a list of the sizes of the storage volumes that SageMaker attaches to each instance, see [Host instance storage volumes](host-instance-storage.md)\.
Endpoints are scoped to an individual AWS account, and are not public\. The URL does not contain the account ID, but SageMaker determines the account ID from the authentication token that is supplied by the caller\.  
For an example of how to use Amazon API Gateway and AWS Lambda to set up and deploy a web service that you can call from a client application that is not within the scope of your account, see [Call a SageMaker model endpoint using Amazon API Gateway and AWS Lambda](https://aws.amazon.com/blogs/machine-learning/call-an-amazon-sagemaker-model-endpoint-using-amazon-api-gateway-and-aws-lambda/) in the *AWS Machine Learning Blog*\.

## Before you begin<a name="realtime-endpoints-deployment-setup"></a>

Before you create and deploy a SageMaker model, locate and make note of the:
+ AWS region where your Amazon S3 bucket is located
+ Amazon S3 URI path where the model artifacts are stored
+ IAM role for SageMaker
+ Docker Amazon ECR URI registry path for the custome image that contains the inference code, or the framework and version of a built\-in Docker image that is supported and by AWS\.

 For a list of AWS Services available in each AWS Region, see [Region Maps and Edge Networks](http://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\. See [Creating IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) for information on how to create an IAM role\.

**Important**  
The Amazon S3 bucket where the model artifacts are stored must be in the same region as the model that you are creating\.

The following demonstrates how to define the aforementioned parameters programmatically\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Import `Boto3`\. Define your AWS region\. Next, initialize a low\-level SageMaker service client object using the `Client` class\. This will make it easy to send and receive requests to AWS services\. In addition, define the AWS role that gives SageMaker permission to access AWS services on your behalf:

```
import boto3

# Specify your AWS Region
aws_region='<aws_region>'

# Create a low-level SageMaker service client.
sagemaker_client = boto3.client('sagemaker', region_name=aws_region)

# Role to give SageMaker permission to access AWS services.
sagemaker_role= "arn:aws:iam::<region>:<account>:role/*"
```

The first few lines define:
+ `sagemaker_client`: A low\-level SageMaker client object that makes it easy to send and receive requests to AWS services\.
+ `sagemaker_role`: A string variable with the SageMaker IAM role Amazon Resource Name \(ARN\)\.
+ `aws_region`: A string variable with the name of your AWS region\.

Import the `image_uris` module from the SageMaker Python SDK\. Use the `retrieve()` function to retrieve the Amazon ECR URI for the Docker image matching the given arguments\. Provide both the name of the framework or algorithm \(`framework` field\) along with the version of the framework or algorithm \(`version` field\)\. For a list of built\-in Docker images provided by AWS, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. For more information on how to bring your own image, see [Use Your Own Inference Code](your-algorithms-inference-main.md)\.

```
from sagemaker import image_uris

# Name of the framework or algorithm
framework='<framework>'
#framework='xgboost' # Example

# Version of the framework or algorithm
version = '<version-number>'
#version = '0.90-1' # Example

# Specify an AWS container image. 
container = image_uris.retrieve(region=aws_region, 
                                framework=framework, 
                                version=version)
```

Next, provide the Amazon S3 URI of the pre\-trained model\. This model will be used to create a SageMaker Python SDK `Model.model` object in the next step\. In this example, the full Amazon S3 URI is stored in a string variable `model_url`:

```
# Create a variable w/ the model S3 URI
# First, provide the name of your S3 bucket
s3_bucket = '<your-bucket-name>' 

# Specify what directory within your S3 bucket your model is stored in
bucket_prefix = '<directory-name>'

# Replace with the name of your model artifact
model_filename = '<model-name>.tar.gz'


# Relative S3 path
model_s3_key = f'{bucket_prefix}/'+model_filename

# Combine bucket name, model file name, and relate S3 path to create S3 model URI
model_url = f's3://{s3_bucket}/{model_s3_key}'
```

------
#### [ SageMaker Python SDK ]

Define your AWS region\. In addition, define the AWS role that gives SageMaker permission to access AWS services on your behalf:

```
# Specify your AWS Region
aws_region='<aws_region>'

# Role to give SageMaker permission to access AWS services.
sagemaker_role= "arn:aws:iam::<region>:<account>:role/*"
```

The first few lines define:
+ `sagemaker_role`: A string variable with the SageMaker IAM role Amazon Resource Name \(ARN\)\.
+ `aws_region`: A string variable with the name of your AWS region\.

Import the `image_uris` module from the SageMaker Python SDK\. Use the `retrieve()` function to retrieve the Amazon ECR URI for the Docker image matching the given arguments\. Provide both the name of the framework or algorithm \(`framework` field\) along with the version of the framework or algorithm \(`version` field\)\. For a list of built\-in Docker images provided by AWS, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. For more information on how to bring your own image, see [Use Your Own Inference Code](your-algorithms-inference-main.md)\.

```
from sagemaker import image_uris

# Name of the framework or algorithm
framework='<framework>'
#framework='xgboost' # Example

# Version of the framework or algorithm
version = '<version-number>'
#version = '0.90-1' # Example

# Specify an AWS container image. 
container = image_uris.retrieve(region=aws_region, 
                                framework=framework, 
                                version=version)
```

Next, provide the Amazon S3 URI of the pre\-trained model\. This model will be used to create a SageMaker Python SDK `Model.model` object in the next step\. In this example, the full Amazon S3 URI is stored in a string variable `model_url`:

```
# Create a variable w/ the model S3 URI
# First, provide the name of your S3 bucket
s3_bucket = '<your-bucket-name>' 

# Specify what directory within your S3 bucket your model is stored in
bucket_prefix = '<directory-name>'

# Replace with the name of your model artifact
model_filename = '<model-name>.tar.gz'


# Relative S3 path
model_s3_key = f'{bucket_prefix}/'+model_filename

# Combine bucket name, model file name, and relate S3 path to create S3 model URI
model_url = f's3://{s3_bucket}/{model_s3_key}'
```

------

## Create a Model<a name="realtime-endpoints-deployment-create-model"></a>

By creating a model, you tell SageMaker where it can find the model components\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Create a model in SageMaker with `CreateModel`\. Specify the following:
+ `ModelName`: A name for your model \(in this example it is stored as a string variable called `model_name`\)\.
+ `ExecutionRoleArn`: The Amazon Resource Name \(ARN\) of the IAM role that Amazon SageMaker can assume to access model artifacts and Docker images for deployment on ML compute instances or for batch transform jobs\.
+ `PrimaryContainer`: The location of the primary Docker image containing inference code, associated artifacts, and custom environment maps that the inference code uses when the model is deployed for predictions\.

For the primary container, specify the Docker image that contains inference code, artifacts \(from prior training\), and a custom environment map that the inference code uses when you deploy the model for predictions\.

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

------
#### [ SageMaker Python SDK ]

Create a model object from the SageMaker Python SDK `Model` Class\. You will use this object to deploy your model to an endpoint in a later step\.

```
from sagemaker.model import Model

model = Model(image_uri=container, 
              model_data=model_url, 
              role=sagemaker_role)
```

------

## Create an Endpoint Configuration<a name="realtime-endpoints-deployment-create-endpoint-config"></a>

Create an endpoint configuration for an HTTPS endpoint\. Specify the name of one or more models in production \(variants\) and the ML compute instances that you want SageMaker to launch to host each production variant\.

**Note**  
Skip this section if you are exclusively using the SageMaker Python SDK\.

When hosting models in production, you can configure the endpoint to elastically scale the deployed ML compute instances\. For each production variant, you specify the number of ML compute instances that you want to deploy\. When you specify two or more instances, SageMaker launches them in multiple Availability Zones\. This ensures continuous availability\. SageMaker manages deploying the instances\. 

------
#### [ AWS SDK for Python \(Boto3\) ]

Create an endpoint configuration with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)\. Amazon SageMaker hosting services uses this configuration to deploy models\. In the configuration, you identify one or more models, created using with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html), to deploy the resources that you want Amazon SageMaker to provision\.

The following example shows how to create an endpoint configuration using AWS SDK for Python \(Boto3\):

```
import datetime
from time import gmtime, strftime

# Create an endpoint config name. Here we create one based on the date  
# so it we can search endpoints based on creation time.
endpoint_config_name = '<endpoint-config-name>'

# The name of the model that you want to host. This is the name that you specified when creating the model.
model_name='<The_name_of_your_model>'
                            
                            
instance_type = '<instance-type>'
# instance_type='ml.m5.xlarge' # Example                            

endpoint_config_response = sagemaker_client.create_endpoint_config(
    EndpointConfigName=endpoint_config_name, # You will specify this name in a CreateEndpoint request.
    # List of ProductionVariant objects, one for each model that you want to host at this endpoint.
    ProductionVariants=[
        {
            "VariantName": "variant1", # The name of the production variant.
            "ModelName": model_name, 
            "InstanceType": instance_type, # Specify the compute instance type.
            "InitialInstanceCount": 1 # Number of instances to launch initially.
        }
    ]
)

print(f"Created EndpointConfig: {endpoint_config_response['EndpointConfigArn']}")
```

In the aforementioned example, you specify the following keys for the `ProductionVariants` field:
+ `VariantName`: The name of the production variant\.
+ `ModelName`: The name of the model that you want to host\. This is the name that you specified when creating the model\.
+ `InstanceType`: The compute instance type\. See the `InstanceType` field in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html) and [SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) for a list of supported compute instance types and pricing for each instance type\.

------
#### [ SageMaker Python SDK ]

You do not need to define an endpoint configuration for real\-time endpoints if you use the SageMaker Python SDK `Model` Class\.

------

## Deploy your model<a name="w2416aac27c18b9b9b9c29"></a>

Deploy your model and create an HTTPS endpoint\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Provide the endpoint configuration to SageMaker\. The service launches the ML compute instances and deploys the model or models as specified in the configuration\.

Once you have your model and endpoint configuration, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API to create your endpoint\. The endpoint name must be unique within an AWS Region in your AWS account\. 

The following creates an endpoint using the endpoint configuration specified in the request\. Amazon SageMaker uses the endpoint to provision resources and deploy models\.

```
# The name of the endpoint. The name must be unique within an AWS Region in your AWS account.
endpoint_name = '<endpoint-name>' 

# The name of the endpoint configuration associated with this endpoint.
endpoint_config_name='<endpoint-config-name>'

create_endpoint_response = sagemaker_client.create_endpoint(
                                            EndpointName=endpoint_name, 
                                            EndpointConfigName=endpoint_config_name)
```

For more information, see the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API\.

------
#### [ SageMaker Python SDK ]

Define a name for your endpoint\. This step is optional\. If you do not provide one, SageMaker will create a unique name for you:

```
from datetime import datetime

endpoint_name = f"DEMO-{datetime.utcnow():%Y-%m-%d-%H%M}"
print("EndpointName =", endpoint_name)
```

Deploy your model to a real\-time, HTTPS endpoint with the Model object’s built\-in `deploy()` method\. Provide the name of the Amazon EC2 instance type to deploy this model to in the `instance_type` field along with the initial number of instances to run the endpoint on for the `initial_instance_count` field:

```
initial_instance_count=<integer>
# initial_instance_count=1 # Example

instance_type='<instance-type>'
# instance_type='ml.m4.xlarge' # Example

model.deploy(
    initial_instance_count=initial_instance_count,
    instance_type=instance_type,
    endpoint_name=endpoint_name
)
```

------