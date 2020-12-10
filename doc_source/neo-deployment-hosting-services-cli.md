# Deploy a Compiled Model Using the AWS CLI<a name="neo-deployment-hosting-services-cli"></a>

You must satisfy the [ prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-prerequisites) section if the model was compiled using AWS SDK for Python \(Boto3\), AWS CLI, or the Amazon SageMaker console\. Follow the steps below to create and deploy a SageMaker Neo\-compiled model using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/)\. 

**Topics**
+ [Deploy the Model](#neo-deploy-cli)

## Deploy the Model<a name="neo-deploy-cli"></a>

After you have satisfied the [ prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-prerequisites), use the `create-model`, `create-enpoint-config`, and `create-endpoint` AWS CLI commands\. The following steps explain how to use these commands to deploy a model compiled with Neo: 

### Create a Model<a name="neo-deployment-hosting-services-cli-create-model"></a>

From [Neo Inference Container Images](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-container-images.html), select the inference image URI and then use `create-model` API to create a SageMaker model\. You can do this with two steps: 

1. Create a `create_model.json` file\. Within the file, specify the name of the model, the image URI, the path to the `model.tar.gz` file in your Amazon S3 bucket, and your SageMaker execution role: 

   ```
   {
           "ModelName": "insert model name",
           "PrimaryContainer": {
           "Image": "insert the ECR Image URI",
           "ModelDataUrl": "insert S3 archive URL",
           "Environment": {"See details below"}
           },
           "ExecutionRoleArn": "ARN for AmazonSageMaker-ExecutionRole"
           }
   ```

   If you trained your model using SageMaker, specify the following environment variable: 

   ```
   "Environment": {
           "SAGEMAKER_SUBMIT_DIRECTORY" : "[Full S3 path for *.tar.gz file containing the training script]"
           }
   ```

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
**Note**  
The `AmazonSageMakerFullAccess` and `AmazonS3ReadOnlyAccess` policies must be attached to the `AmazonSageMaker-ExecutionRole` IAM role\. 

1. Run the following command:

   ```
   aws sagemaker create-model --cli-input-json file://create_model.json
   ```

   For the full syntax of the `create-model` API, see [https://docs.aws.amazon.com/cli/latest/reference/sagemaker/create-model.html](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/create-model.html)\. 

### Create an Endpoint Configuration<a name="neo-deployment-hosting-services-cli-create-endpoint-config"></a>

After creating a SageMaker model, create the endpoint configuration using the `create-endpoint-config` API\. To do this, create a JSON file with your endpoint configuration specifications\. For example, you can use the following code template and save it as `create_config.json`: 

```
{
        "EndpointConfigName": "<provide your endpoint config name>",
        "ProductionVariants": [
        {
        "VariantName": "<provide your variant name>",
        "ModelName": "my-sagemaker-model",
        "InitialInstanceCount": 1,
        "InstanceType": "<provide your instance type here>",
        "InitialVariantWeight": 1.0
        }
        ]
        }
```

Now run the following AWS CLI command to create your endpoint configuration: 

```
aws sagemaker create-endpoint-config --cli-input-json file://create_config.json
```

For the full syntax of the `create-endpoint-config` API, see [ `create-endpoint-config`](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/create-endpoint-config.html)\. 

### Create an Endpoint<a name="neo-deployment-hosting-services-cli-create-endpoint"></a>

After you have created your endpoint configuration, create an endpoint using the `create-endpoint` API: 

```
aws sagemaker create-endpoint --endpoint-name '<provide your endpoint name>' --endpoint-config-name '<insert your endpoint config name>'
```

For the full syntax of the `create-endpoint` API, see [https://docs.aws.amazon.com/cli/latest/reference/sagemaker/create-endpoint.html](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/create-endpoint.html)\. 