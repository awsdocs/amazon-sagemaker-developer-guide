# Deploying large models with Amazon SageMaker<a name="realtime-endpoints-large-model-hosting"></a>

State\-of\-the\-art natural language processing \(NLP\) models are large, typically with billions of parameters\. Empirical demonstrations show that larger models are more accurate\. But these models are often too large to fit on single accelerators, making low\-latency inference with them difficult\. With SageMaker Hosting you can customize the following parameters to facilitate low\-latency real\-time inference with large models:
+ Maximum Amazon EBS volume size on the instance
+ Health check timeout quota
+ Model download timeout quota

For more information on low latency inference with large models, see [ Deploy large models on Amazon SageMaker using DJLServing and DeepSpeed model parallel inference](http://aws.amazon.com/blogs/machine-learning/deploy-large-models-on-amazon-sagemaker-using-djlserving-and-deepspeed-model-parallel-inference/)\. The following code snippet demonstrates how to programatically configure the aforementioned parameters\. Replace the *italicized placeholder text* in the example with your own information\.

```
import boto3

aws_region = '<aws_region>'
sagemaker_client = boto3.client('sagemaker', region_name=aws_region)

# The name of the endpoint. The name must be unique within an AWS Region in your AWS account.
endpoint_name = '<endpoint-name>'

# Create an endpoint config name.
endpoint_config_name = '<endpoint-config-name>'

# The name of the model that you want to host.
model_name = '<the-name-of-your-model>'

instance_type = '<instance-type>'

sagemaker_client.create_endpoint_config(
    EndpointConfigName = endpoint_config_name
    ProductionVariants=[
        {
	    "VariantName": 'variant1', # The name of the production variant.
	    "ModelName": model_name,
	    "InstanceType": instance_type, # Specify the compute instance type.
	    "InitialInstanceCount": 1, # Number of instances to launch initially.
	    "VolumeSizeInGB": 256, # Specify the size of the Amazon EBS volume.
	    "ModelDataDownloadTimeoutInSeconds": 1800, # Specify the model download timeout in seconds.
	    "ContainerStartupHealthCheckTimeoutInSeconds": 1800, # Specify the health checkup timeout in seconds
	},
    ],
)

sagemaker_client.create_endpoint(EndpointName=endpoint_name, EndpointConfigName=endpoint_config_name)
```

Check [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html) and [Create an Endpoint Configuration](realtime-endpoints-deployment.md#realtime-endpoints-deployment-create-endpoint-config) for more information on the keys for `ProductionVariants`\.