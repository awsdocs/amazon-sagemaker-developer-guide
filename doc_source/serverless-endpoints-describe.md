# Describe a serverless endpoint<a name="serverless-endpoints-describe"></a>


****  

|  | 
| --- |
| Serverless Inference is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

You might want to retrieve information about your endpoint, including details such as the endpoint’s ARN, current status, deployment configuration, and failure reasons\. You can find information about your endpoint using the [SageMaker console](https://console.aws.amazon.com/sagemaker/home), the [DescribeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html) API, or the AWS CLI\.

## To describe an endpoint \(API\)<a name="serverless-endpoints-describe-api"></a>

The following example uses the [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#id309) to call the [DescribeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html) API\. For `EndpointName`, use the name of the endpoint you want to check\.

```
response = client.describe_endpoint(
    EndpointName="<your-endpoint-name>",
)
```

## To describe an endpoint \(console\)<a name="serverless-endpoints-describe-console"></a>

1. Sign in to the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home)\.

1. In the navigation tab, choose **Inference**\.

1. Next, choose **Endpoints**\.

1. From the list of endpoints, choose the endpoint you want to check\.

The endpoint page contains the information about your endpoint\.