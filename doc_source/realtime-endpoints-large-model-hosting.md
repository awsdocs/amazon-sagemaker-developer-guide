# SageMaker endpoint parameters for large model inference<a name="realtime-endpoints-large-model-hosting"></a>

 You can customize the following parameters to facilitate low\-latency LMI with SageMaker: 
+  **Maximum Amazon EBS volume size on the instance \(`VolumeSizeInGB`\)** – If the size of the model is larger than 30 GB and you are using an instance without a local disk, you should increase this parameter to be slightly larger than the size of your model\. 
+  **Health check timeout quota \(`ContainerStartupHealthCheckTimeoutInSeconds`\)** – If your container is correctly setup and the CloudWatch logs indicate a health check timeout, you should increase this quota so the container has enough time to respond to health checks\. 
+  **Model download timeout quota \(`ModelDataDownloadTimeoutInSeconds`\)** – If the size of your model is larger than 40 GB, then you should increase this quota to provide sufficient time to download the model from Amazon S3 to the instance\. 

 For more information on low latency inference with large models, see [ Deploy large models on Amazon SageMaker using DJL Serving and DeepSpeed model parallel inference](http://aws.amazon.com/blogs/machine-learning/deploy-large-models-on-amazon-sagemaker-using-djlserving-and-deepspeed-model-parallel-inference/)\. The following code snippet demonstrates how to programatically configure the aforementioned parameters\. Replace the *italicized placeholder text* in the example with your own information\. 

```
import boto3

aws_region = "aws-region"
sagemaker_client = boto3.client('sagemaker', region_name=aws_region)

# The name of the endpoint. The name must be unique within an AWS Region in your AWS account.
endpoint_name = "endpoint-name"

# Create an endpoint config name.
endpoint_config_name = "endpoint-config-name"

# The name of the model that you want to host.
model_name = "the-name-of-your-model"

instance_type = "instance-type"

sagemaker_client.create_endpoint_config(
    EndpointConfigName = endpoint_config_name
    ProductionVariants=[
        {
            "VariantName": "variant1", # The name of the production variant.
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

 For more information on the keys for `ProductionVariants`, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html) and [Create an Endpoint Configuration](realtime-endpoints-deployment.md#realtime-endpoints-deployment-create-endpoint-config)\. 