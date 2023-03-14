# Delete Endpoints and Resources<a name="realtime-endpoints-delete-resources"></a>

Delete endpoints to stop incurring charges\.

## Delete Endpoint<a name="realtime-endpoints-delete-endpoint"></a>

Delete your endpoint programmatically using AWS SDK for Python \(Boto3\), with the AWS CLI, or interactively using the SageMaker console\.

SageMaker frees up all of the resources that were deployed when the endpoint was created\. Deleting an endpoint will not delete the endpoint configuration or the SageMaker model\. See [Delete Endpoint Configuration](#realtime-endpoints-delete-endpoint-config) and [Delete Model](#realtime-endpoints-delete-model) for information on how to delete your endpoint configuration and SageMaker model\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpoint.html) API to delete your endpoint\. Specify the name of your endpoint for the `EndpointName` field\.

```
import boto3

# Specify your AWS Region
aws_region='<aws_region>'

# Specify the name of your endpoint
endpoint_name='<endpoint_name>'

# Create a low-level SageMaker service client.
sagemaker_client = boto3.client('sagemaker', region_name=aws_region)

# Delete endpoint
sagemaker_client.delete_endpoint(EndpointName=endpoint_name)
```

------
#### [ AWS CLI ]

Use the [https://docs.aws.amazon.com/cli/latest/reference/sagemaker/delete-endpoint.html](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/delete-endpoint.html) command to delete your endpoint\. Specify the name of your endpoint for the `endpoint-name` flag\.

```
aws sagemaker delete-endpoint --endpoint-name <endpoint-name>
```

------
#### [ SageMaker Console ]

Delete your endpoint interactively with the SageMaker console\.

1. In the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) navigation menu, choose **Inference**\.

1. Choose **Endpoints** from the drop down menu\. A list of endpoints created in you AWS account will appear by name, Amazon Resource Name \(ARN\), creation time, status, and a time stamp of when the endpoint was last updated\.

1. Select the endpoint you want to delete\.

1. Select the **Actions** dropdown button in the top right corner\.

1. Choose **Delete**\.

------

## Delete Endpoint Configuration<a name="realtime-endpoints-delete-endpoint-config"></a>

Delete your endpoint configuration programmaticially using AWS SDK for Python \(Boto3\), with the AWS CLI, or interactively using the SageMaker console\. Deleting an endpoint configuration does not delete endpoints created using this configuration\. See [Delete Endpoint](#realtime-endpoints-delete-endpoint) for information on how to delete your endpoint\.

Do not delete an endpoint configuration in use by an endpoint that is live or while the endpoint is being updated or created\. You might lose visibility into the instance type the endpoint is using if you delete the endpoint configuration of an endpoint that is active or being created or updated\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpointConfig.html) API to delete your endpoint\. Specify the name of your endpoint configuration for the `EndpointConfigName` field\.

```
import boto3

# Specify your AWS Region
aws_region='<aws_region>'

# Specify the name of your endpoint configuration
endpoint_config_name='<endpoint_name>'

# Create a low-level SageMaker service client.
sagemaker_client = boto3.client('sagemaker', region_name=aws_region)

# Delete endpoint configuration
sagemaker_client.delete_endpoint_config(EndpointConfigName=endpoint_config_name)
```

You can optionally use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html) API to return information about the name of the your deployed models \(production variants\) such as the name of your model and the name of the endpoint configuration associated with that deployed model\. Provide the name of your endpoint for the `EndpointConfigName` field\. 

```
# Specify the name of your endpoint
endpoint_name='<endpoint_name>'

# Create a low-level SageMaker service client.
sagemaker_client = boto3.client('sagemaker', region_name=aws_region)

# Store DescribeEndpointConfig response into a variable that we can index in the next step.
response = sagemaker_client.describe_endpoint_config(EndpointConfigName=endpoint_name)

# Delete endpoint
endpoint_config_name = response['ProductionVariants'][0]['EndpointConfigName']
                        
# Delete endpoint configuration
sagemaker_client.delete_endpoint_config(EndpointConfigName=endpoint_config_name)
```

For more information about other response elements returned by `DescribeEndpointConfig`, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html) in the [SageMaker API Reference guide](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Operations_Amazon_SageMaker_Service.html)\.

------
#### [ AWS CLI ]

Use the [https://docs.aws.amazon.com/cli/latest/reference/sagemaker/delete-endpoint-config.html](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/delete-endpoint-config.html) command to delete your endpoint configuration\. Specify the name of your endpoint configuration for the `endpoint-config-name` flag\.

```
aws sagemaker delete-endpoint-config \
                        --endpoint-config-name <endpoint-config-name>
```

You can optionally use the [https://docs.aws.amazon.com/cli/latest/reference/sagemaker/describe-endpoint-config.html](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/describe-endpoint-config.html) command to return information about the name of the your deployed models \(production variants\) such as the name of your model and the name of the endpoint configuration associated with that deployed model\. Provide the name of your endpoint for the `endpoint-config-name` flag\.

```
aws sagemaker describe-endpoint-config --endpoint-config-name <endpoint-config-name>
```

This will return a JSON response\. You can copy and paste, use a JSON parser, or use a tool built for JSON parsing to obtain the endpoint configuration name associated with that endpoint\.

------
#### [ SageMaker Console ]

Delete your endpoint configuration interactively with the SageMaker console\.

1. In the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) navigation menu, choose **Inference**\.

1. Choose **Endpoint configurations** from the dropdown menu\. A list of endpoint configurations created in you AWS account will appear by name, Amazon Resource Name \(ARN\), and creation time\.

1. Select the endpoint configuration you want to delete\.

1. Select the **Actions** dropdown button in the top right corner\.

1. Choose **Delete**\.

------

## Delete Model<a name="realtime-endpoints-delete-model"></a>

Delete your SageMaker model programmaticially using AWS SDK for Python \(Boto3\), with the AWS CLI, or interactively using the SageMaker console\. Deleting a SageMaker model only deletes the model entry that was created in SageMaker\. Deleting a model does not delete model artifacts, inference code, or the IAM role that you specified when creating the model\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteModel.html) API to delete your SageMaker model\. Specify the name of your model for the `ModelName` field\.

```
import boto3

# Specify your AWS Region
aws_region='<aws_region>'

# Specify the name of your endpoint configuration
model_name='<model_name>'

# Create a low-level SageMaker service client.
sagemaker_client = boto3.client('sagemaker', region_name=aws_region)

# Delete model
sagemaker_client.delete_model(ModelName=model_name)
```

You can optionally use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html) API to return information about the name of the your deployed models \(production variants\) such as the name of your model and the name of the endpoint configuration associated with that deployed model\. Provide the name of your endpoint for the `EndpointConfigName` field\. 

```
# Specify the name of your endpoint
endpoint_name='<endpoint_name>'

# Create a low-level SageMaker service client.
sagemaker_client = boto3.client('sagemaker', region_name=aws_region)

# Store DescribeEndpointConfig response into a variable that we can index in the next step.
response = sagemaker_client.describe_endpoint_config(EndpointConfigName=endpoint_name)

# Delete endpoint
model_name = response['ProductionVariants'][0]['ModelName']
sagemaker_client.delete_model(ModelName=model_name)
```

For more information about other response elements returned by `DescribeEndpointConfig`, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html) in the [SageMaker API Reference guide](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Operations_Amazon_SageMaker_Service.html)\.

------
#### [ AWS CLI ]

Use the [https://docs.aws.amazon.com/cli/latest/reference/sagemaker/delete-model.html](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/delete-model.html) command to delete your SageMaker model\. Specify the name of your model for the `model-name` flag\.

```
aws sagemaker delete-model \
                        --model-name <model-name>
```

You can optionally use the [https://docs.aws.amazon.com/cli/latest/reference/sagemaker/describe-endpoint-config.html](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/describe-endpoint-config.html) command to return information about the name of the your deployed models \(production variants\) such as the name of your model and the name of the endpoint configuration associated with that deployed model\. Provide the name of your endpoint for the `endpoint-config-name` flag\.

```
aws sagemaker describe-endpoint-config --endpoint-config-name <endpoint-config-name>
```

This will return a JSON response\. You can copy and paste, use a JSON parser, or use a tool built for JSON parsing to obtain the name of the model associated with that endpoint\.

------
#### [ SageMaker Console ]

Delete your SageMaker model interactively with the SageMaker console\.

1. In the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) navigation menu, choose **Inference**\.

1. Choose **Models** from the dropdown menu\. A list of models created in you AWS account will appear by name, Amazon Resource Name \(ARN\), and creation time\.

1. Select the model you want to delete\.

1. Select the **Actions** dropdown button in the top right corner\.

1. Choose **Delete**\.

------