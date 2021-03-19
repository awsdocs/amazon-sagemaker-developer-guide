# Deploy a Compiled Model Using Boto3<a name="neo-deployment-hosting-services-boto3"></a>

You must satisfy the [ prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-prerequisites) section if the model was compiled using AWS SDK for Python \(Boto3\), AWS CLI, or the Amazon SageMaker console\. Follow the steps below to create and deploy a SageMaker Neo\-compiled model using [Amazon Web Services SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)\. 

**Topics**
+ [Deploy the Model](#neo-deployment-hosting-services-boto3-steps)

## Deploy the Model<a name="neo-deployment-hosting-services-boto3-steps"></a>

After you have satisfied the [ prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-prerequisites), use the `create_model`, `create_enpoint_config`, and `create_endpoint` APIs\. 

The following example shows how to use these APIs to deploy a model compiled with Neo: 

```
import boto3
client = boto3.client('sagemaker')

# create sagemaker model
create_model_api_response = client.create_model(
                                ModelName='my-sagemaker-model',
                                    PrimaryContainer={
                                        'Image': <insert the ECR Image URI>,
                                        'ModelDataUrl': 's3://path/to/model/artifact/model.tar.gz',
                                        'Environment': {}
                                    },
                               ExecutionRoleArn='ARN for AmazonSageMaker-ExecutionRole'
                            )

print ("create_model API response", create_model_api_response)

# create sagemaker endpoint config
create_endpoint_config_api_response = client.create_endpoint_config(
                                            EndpointConfigName='sagemaker-neomxnet-endpoint-configuration',
                                            ProductionVariants=[
                                                {
                                                    'VariantName': <provide your variant name>,
                                                    'ModelName': 'my-sagemaker-model',
                                                    'InitialInstanceCount': 1,
                                                    'InstanceType': <provide your instance type here>
                                                },
                                            ]
                                        )

print ("create_endpoint_config API response", create_endpoint_config_api_response)

# create sagemaker endpoint
create_endpoint_api_response = client.create_endpoint(
                                    EndpointName='provide your endpoint name',
                                    EndpointConfigName=<insert your endpoint config name>,
                                )

print ("create_endpoint API response", create_endpoint_api_response)
```

**Note**  
The `AmazonSageMakerFullAccess` and `AmazonS3ReadOnlyAccess` policies must be attached to the `AmazonSageMaker-ExecutionRole` IAM role\. 

For full syntax of `create_model`, `create_endpoint_config`, and `create_endpoint` APIs, see [ `create_model`](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model), [ `create_endpoint_config`](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_endpoint_config), and [ `create_endpoint`](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_endpoint), respectively\. 

If you did not train your model using SageMaker, specify the following environment variables: 

------
#### [ MXNet and PyTorch ]

```
"Environment": {
    "SAGEMAKER_PROGRAM": "inference.py",
    "SAGEMAKER_SUBMIT_DIRECTORY": "/opt/ml/model/code",
    "SAGEMAKER_CONTAINER_LOG_LEVEL": "20",
    "SAGEMAKER_REGION": "insert your region",
    "MMS_DEFAULT_RESPONSE_TIMEOUT": "500"
}
```

------
#### [ TensorFlow ]

```
"Environment": {
    "SAGEMAKER_PROGRAM": "inference.py",
    "SAGEMAKER_SUBMIT_DIRECTORY": "/opt/ml/model/code",
    "SAGEMAKER_CONTAINER_LOG_LEVEL": "20",
    "SAGEMAKER_REGION": "insert your region"
}
```

------

 If you trained your model using SageMaker, specify the environment variable `SAGEMAKER_SUBMIT_DIRECTORY` as the full Amazon S3 bucket URI that contains the training script\. 