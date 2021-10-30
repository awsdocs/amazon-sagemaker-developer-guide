# Real\-time Inference<a name="realtime-endpoints"></a>

Real\-time inference is ideal for inference workloads where you have real\-time, interactive, low latency requirements\. You can deploy your model to SageMaker hosting services and get an endpoint that can be used for inference\. These endpoints are fully managed, support autoscaling \(see [Automatically Scale Amazon SageMaker Models](endpoint-auto-scaling.md)\), and can be deployed in multiple [Availability Zones](instance-types-az.md)\.

## Deploy Your Model<a name="realtime-endpoints-deployment"></a>

Deploying a model using SageMaker hosting services is a three\-step process:

1. **Create a model in SageMaker**—By creating a model, you tell SageMaker where it can find the model components\. This includes the S3 path where the model artifacts are stored and the Docker registry path for the image that contains the inference code\. In subsequent deployment steps, you specify the model by name\. For more information, see the [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) API\.
**Note**  
The S3 bucket where the model artifacts are stored must be in the same region as the model that you are creating\.

1. **Create an endpoint configuration for an HTTPS endpoint**—You specify the name of one or more models in production variants and the ML compute instances that you want SageMaker to launch to host each production variant\.
**Note**  
 SageMaker supports running \(a\) multiple models, \(b\) multiple variants of a model, or \(c\) combinations of models and variants on the same endpoint\. Model variants can reference the same model inference container \(i\.e\. run the same algorithm\), but use different model artifacts \(e\.g\., different model weight values based on other hyper\-parameter configurations\)\. In contrast, two different models may use the same algorithm, but focus on different business problems or underlying goals and may operate on different data sets\. 

   When hosting models in production, you can configure the endpoint to elastically scale the deployed ML compute instances\. For each production variant, you specify the number of ML compute instances that you want to deploy\. When you specify two or more instances, SageMaker launches them in multiple Availability Zones\. This ensures continuous availability\. SageMaker manages deploying the instances\. For more information, see the [ `CreateEndpointConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) API\.

1. **Create an HTTPS endpoint**—Provide the endpoint configuration to SageMaker\. The service launches the ML compute instances and deploys the model or models as specified in the configuration\. For more information, see the [ `CreateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API\. To get inferences from the model, client applications send requests to the SageMaker Runtime HTTPS endpoint\. For more information about the API, see the [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html) API\. 
**Note**  
Endpoints are scoped to an individual AWS account, and are not public\. The URL does not contain the account ID, but SageMaker determines the account ID from the authentication token that is supplied by the caller\.  
For an example of how to use Amazon API Gateway and AWS Lambda to set up and deploy a web service that you can call from a client application that is not within the scope of your account, see [Call a SageMaker model endpoint using Amazon API Gateway and AWS Lambda](https://aws.amazon.com/blogs/machine-learning/call-an-amazon-sagemaker-model-endpoint-using-amazon-api-gateway-and-aws-lambda/) in the *AWS Machine Learning Blog*\.

**Note**  
When you create an endpoint, SageMaker attaches an Amazon EBS storage volume to each ML compute instance that hosts the endpoint\. The size of the storage volume depends on the instance type\. For a list of instance types that SageMaker hosting service supports, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_sagemaker)\. For a list of the sizes of the storage volumes that SageMaker attaches to each instance, see [Host Instance Storage Volumes](host-instance-storage.md)\.

To increase a model's accuracy, you might choose to save the user's input data and ground truth, if available, as part of the training data\. You can then retrain the model periodically with a larger, improved training dataset\.