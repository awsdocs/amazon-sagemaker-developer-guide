# Create your endpoint and deploy your model<a name="realtime-endpoints-deployment"></a>

Deploying a model using SageMaker hosting services is a three\-step process:

1. **Create a model in SageMaker**—By creating a model, you tell SageMaker where it can find the model components\. This includes the S3 path where the model artifacts are stored and the Docker registry path for the image that contains the inference code\. In subsequent deployment steps, you specify the model by name\. For more information, see the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) API\.
**Note**  
The S3 bucket where the model artifacts are stored must be in the same region as the model that you are creating\.

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
   sagemaker_role= "arn:aws:iam::<region>:<account>:role/*"
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
   from sagemaker.amazon.amazon_estimator import get_image_uri
   
   # Specify an AWS container image. 
   container = get_image_uri(region=aws_region, framework='xgboost', version='0.90-1')
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

1. **Create an endpoint configuration for an HTTPS endpoint**—You specify the name of one or more models in production variants and the ML compute instances that you want SageMaker to launch to host each production variant\.
**Note**  
 SageMaker supports running \(a\) multiple models, \(b\) multiple variants of a model, or \(c\) combinations of models and variants on the same endpoint\. Model variants can reference the same model inference container \(i\.e\. run the same algorithm\), but use different model artifacts \(e\.g\., different model weight values based on other hyper\-parameter configurations\)\. In contrast, two different models may use the same algorithm, but focus on different business problems or underlying goals and may operate on different data sets\. 

   When hosting models in production, you can configure the endpoint to elastically scale the deployed ML compute instances\. For each production variant, you specify the number of ML compute instances that you want to deploy\. When you specify two or more instances, SageMaker launches them in multiple Availability Zones\. This ensures continuous availability\. SageMaker manages deploying the instances\. 

   Once you have a model, create an endpoint configuration with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)\. Amazon SageMaker hosting services uses this configuration to deploy models\. In the configuration, you identify one or more models, created using with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html), to deploy the resources that you want Amazon SageMaker to provision\.

   The following example shows how to create an endpoint configuration using AWS SDK for Python \(Boto3\):

   ```
   import datetime
   from time import gmtime, strftime
   
   # Create an endpoint config name. Here we create one based on the date  
   # so it we can search endpoints based on creation time.
   endpoint_config_name = f"XGBoostEndpointConfig-{strftime('%Y-%m-%d-%H-%M-%S', gmtime())}"
   
   # The name of the model that you want to host. This is the name that you specified when creating the model.
   model_name='<The_name_of_your_model>'
   
   endpoint_config_response = sagemaker_client.create_endpoint_config(
       EndpointConfigName=endpoint_config_name, # You will specify this name in a CreateEndpoint request.
       # List of ProductionVariant objects, one for each model that you want to host at this endpoint.
       ProductionVariants=[
           {
               "VariantName": "variant1", # The name of the production variant.
               "ModelName": model_name, 
               "InstanceType": "ml.m5.xlarge", # Specify the compute instance type.
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

   For more information about required and optional API fields, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) API in the [SageMaker Service API Reference](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Operations_Amazon_SageMaker_Service.html) guide\.

1. **Create an HTTPS endpoint**—Provide the endpoint configuration to SageMaker\. The service launches the ML compute instances and deploys the model or models as specified in the configuration\.
**Note**  
When you create an endpoint, SageMaker attaches an Amazon EBS storage volume to each ML compute instance that hosts the endpoint\. The size of the storage volume depends on the instance type\. For a list of instance types that SageMaker hosting service supports, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_sagemaker)\. For a list of the sizes of the storage volumes that SageMaker attaches to each instance, see [Host instance storage volumes](host-instance-storage.md)\.
Endpoints are scoped to an individual AWS account, and are not public\. The URL does not contain the account ID, but SageMaker determines the account ID from the authentication token that is supplied by the caller\.  
For an example of how to use Amazon API Gateway and AWS Lambda to set up and deploy a web service that you can call from a client application that is not within the scope of your account, see [Call a SageMaker model endpoint using Amazon API Gateway and AWS Lambda](https://aws.amazon.com/blogs/machine-learning/call-an-amazon-sagemaker-model-endpoint-using-amazon-api-gateway-and-aws-lambda/) in the *AWS Machine Learning Blog*\.

   Once you have your model and endpoint configuration, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API to create your endpoint\. The endpoint name must be unique within an AWS Region in your AWS account\. 

   The following creates an endpoint using the endpoint configuration specified in the request\. Amazon SageMaker uses the endpoint to provision resources and deploy models\.

   ```
   # The name of the endpoint. The name must be unique within an AWS Region in your AWS account.
   endpoint_name = '<endpoint-name>' 
   
   # The name of the endpoint configuration associated with this endpoint.
   endpoint_config_name='<endpoint_config_name>'
   
   create_endpoint_response = sagemaker_client.create_endpoint(
                                               EndpointName=endpoint_name, 
                                               EndpointConfigName=endpoint_config_name)
   ```

   For more information, see the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API\.