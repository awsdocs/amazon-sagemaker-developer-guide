# Update a serverless endpoint<a name="serverless-endpoints-update"></a>


****  

|  | 
| --- |
| Serverless Inference is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

Before updating your endpoint, create a new endpoint configuration or use an existing endpoint configuration\. The endpoint configuration is where you specify the changes for your update\. Then, you can update your endpoint with the [SageMaker console](https://console.aws.amazon.com/sagemaker/home), the [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API, or the AWS CLI\. The process for updating a serverless endpoint is the same as the process for updating a [real\-time endpoint](realtime-endpoints.md)\. Note that when updating your endpoint, you can experience cold starts when making requests to the endpoint because SageMaker must re\-initialize your container and model\.

## Create a new endpoint configuration<a name="serverless-endpoints-update-create"></a>

To create a new serverless endpoint configuration, see the previous procedure in [Create an endpoint configuration](serverless-endpoints-create.md#serverless-endpoints-create-config)\. You can also create a real\-time endpoint configuration to switch from serverless to instance\-based capacity\.

## Update the endpoint<a name="serverless-endpoints-update-endpoint"></a>

Examples of how to update your endpoint using the [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API and the [SageMaker console](https://console.aws.amazon.com/sagemaker/home) are outlined in the following sections\.

### To update the endpoint \(API\)<a name="serverless-endpoints-update-endpoint-api"></a>

The following example uses the [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#id309) to call the [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API\. Specify the following values:
+ For `EndpointName`, use the name of the endpoint you’re updating\.
+ For `EndpointConfigName`, use the name of the endpoint configuration that you want to use for the update\.

```
response = client.update_endpoint(
    EndpointName="<your-endpoint-name>",
    EndpointConfigName="<new-endpoint-config>",
)
```

### To update the endpoint \(console\)<a name="serverless-endpoints-update-endpoint-console"></a>

1. Sign in to the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home)\.

1. In the navigation tab, choose **Inference**\.

1. Next, choose **Endpoints**\.

1. From the list of endpoints, select the endpoint you want to update\.

1. Choose **Update endpoint**\.

1. For **Change the Endpoint configuration**, choose **Use an existing endpoint configuration**\.

1. From the list of endpoint configurations, select the one you want to use for your update\.

1. Choose **Select endpoint configuration**\.

1. Choose **Update endpoint**\.