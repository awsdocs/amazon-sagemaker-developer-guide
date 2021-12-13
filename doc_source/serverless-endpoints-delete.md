# Delete a serverless endpoint<a name="serverless-endpoints-delete"></a>


****  

|  | 
| --- |
| Serverless Inference is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

You can delete your serverless endpoint using the [SageMaker console](https://console.aws.amazon.com/sagemaker/home), the [DeleteEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpoint.html) API, or the AWS CLI\. The following examples show you how to delete your endpoint through the API and the SageMaker console\.

## To delete an endpoint \(API\)<a name="serverless-endpoints-delete-api"></a>

The following example uses the [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#id309) to call the [DeleteEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpoint.html) API\. For `EndpointName`, use the name of the serverless endpoint you want to delete\.

```
response = client.delete_endpoint(
    EndpointName="<your-endpoint-name>",
)
```

## To delete an endpoint \(console\)<a name="serverless-endpoints-delete-console"></a>

1. Sign in to the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home)\.

1. In the navigation tab, choose **Inference**\.

1. Next, choose **Endpoints**\.

1. From the list of endpoints, select the endpoint you want to delete\.

1. Choose the **Actions** drop\-down list, and then choose **Delete**\.

1. When prompted again, choose **Delete**\.

Your endpoint should now begin the deletion process\.