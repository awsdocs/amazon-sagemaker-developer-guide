# Invoke a serverless endpoint<a name="serverless-endpoints-invoke"></a>

In order to perform inference using a serverless endpoint, you must send an HTTP request to the endpoint\. You can use the [InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html) API or the AWS CLI, which make a `POST` request to invoke your endpoint\. The maximum request and response payload size for serverless invocations is 4 MB\. Note that a cold start \(or a delay in spinning up compute resources\) on the endpoint may delay the response time to up to 4 minutes:
+ The model must download and the server must respond successfully to `/ping` within 3 minutes\.
+ The timeout for the container to respond to inference requests to `/invocations` is 1 minute\.

For more information on cold starts for serverless endpoints, see [Cold starts](serverless-endpoints.md#serverless-endpoints-how-it-works-cold-starts)\.

## To invoke an endpoint<a name="serverless-endpoints-invoke-api"></a>

The following example uses the [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#id309) to call the [InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html) API\. Note that unlike the other API calls in this guide, for `InvokeEndpoint`, you must use SageMaker Runtime as the client\. Specify the following values:
+ For `endpoint_name`, use the name of the in\-service serverless endpoint you want to invoke\.
+ For `content_type`, specify the MIME type of your input data in the request body \(for example, `application/json`\)\.
+ For `payload`, use your request payload for inference\. Your payload should be in bytes or a file\-like object\.

```
runtime = boto3.client("sagemaker-runtime")

endpoint_name = "<your-endpoint-name>"
content_type = "<request-mime-type>"
payload = <your-request-body>

response = runtime.invoke_endpoint(
    EndpointName=endpoint_name,
    ContentType=content_type,
    Body=payload
)
```