# Test Endpoints<a name="realtime-endpoints-test-endpoints"></a>

After you deploy your model using SageMaker hosting services, you can test your model on that endpoint by sending it test data\. You can test your endpoints using Amazon SageMaker Studio, the AWS CLI, or AWS SDKs\.

## Test Your Endpoint Using Amazon SageMaker Studio<a name="realtime-endpoints-test-endpoints-studio"></a>

After you deploy your model to an endpoint \(see [Create Your Endpoint and Deploy Your Model](realtime-endpoints-deployment.md)\) you can check that endpoint with Amazon SageMaker Studio\.

**Note**  
Note: SageMaker only supports endpoint testing with Amazon SageMaker Studio for real\-time endpoints\.

1. Launch Amazon SageMaker Studio\.

1. Select the **SageMaker Components and registries** icon on the left sidebar\.

1. From the dropdown, choose **Endpoints**\.

1. Search for your endpoint by name and double\-click on the name of your endpoint\. The endpoint names listed within the **SageMaker resources** panel are defined when you deploy a model\. You can deploy your model in several ways:
   + The SageMaker Python SDK Model Class [https://sagemaker.readthedocs.io/en/stable/api/inference/model.html?highlight=deploy#sagemaker.model.Model.deploy](https://sagemaker.readthedocs.io/en/stable/api/inference/model.html?highlight=deploy#sagemaker.model.Model.deploy)\.
   + The AWS SDK for Python \(Boto3\) SageMaker Service Client API [CreateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html)\.
   + The SageMaker console\. Select **Inference** in the left panel and then select **Endpoint configuration**\. Provide an endpoint name within the **Endpoint name** field\.

1. \(Optional\) You can optionally provide a custom URL to send your request to\. In the **Configure endpoint URL and headers** field provide the URL of where your model is hosted\. Leave this field blank if you are using a SageMaker endpoint\. You can also optionally add multiple key\-value headers to pass additional information to the inference request\. 

1. A new tab will populate in the Studio workspace\. Select the **Test inference** tab\.

1. Send a request to your endpoint by providing sample data in JSON format\. Use the JSON editor to submit a request to your endpoint\.

1. Select **Send Request**\.

1. When you send the request an *Inference output card* will appear on the right\-hand side of the console\.

The top of the card will display the type of request that was sent to the endpoint \(currently only JSON is accepted\)\. Within the card there are four main fields: `Status`, `Execution Length`, `Request Time`, and `Result Time`\.
+ **Status**: displays the request status\. There are three types of statuses:
  + `Complete` \- If the request is successful, the status will display `Complete` and the execution length will is calculated by tracking the request time\.
  + `Failed` \- If the request fails for any reason\. A failure response will appear within the **Failure Reason** accordion\.
  + `Pending` \- A spinning, circular icon will appear while the inference request is pending\.
+ **Execution Length**: How long the invocation takes \(end time minus the start time\) in milliseconds\.
+ **Request Time**: How many minutes have passed since the request was sent\.
+ **Result Time**: How many minutes have passed since the result was returned\.

## Test Your Endpoint Using AWS SDKs<a name="realtime-endpoints-test-endpoints-api"></a>

After you deploy your model to an endpoint \(see [Create Your Endpoint and Deploy Your Model](realtime-endpoints-deployment.md#realtime-endpoints-deployment.title)\) you can check your endpoint with the `InvokeEndpoint` API using one of the AWS SDKs\. If the invocation is successful, SageMaker will return an HTTP 200 response\. You can index the returned response dictionary object to find out more about the response headers\. For more information, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html#API_runtime_InvokeEndpoint_ResponseElements](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html#API_runtime_InvokeEndpoint_ResponseElements)\.

The following demonstrates how to use `InvokeEndpoint` to check the status of your endpoint using the AWS SDK for Python \(Boto3\)\. Provide the name of your endpoint\. This is the name you specified for `EndpointName` when you created your endpoint with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html)\.

Provide input data in the Body field for SageMaker to pass to the model\. The data must be in the same format that was used for training\.

```
import boto3

# Create a low-level client representing Amazon SageMaker Runtime
sagemaker_runtime = boto3.client("sagemaker-runtime", region_name=<aws_region>)

# The name of the endpoint. The name must be unique within an AWS Region in your AWS account. 
endpoint_name='<endpoint-name>'

# After you deploy a model into production using SageMaker hosting 
# services, your client applications use this API to get inferences 
# from the model hosted at the specified endpoint.
response = sagemaker_runtime.invoke_endpoint(
                            EndpointName=endpoint_name, 
                            Body=bytes('{"features": ["This is great!"]}', 'utf-8') # Replace with your own data.
                            )

# Optional - Print the response body and decode it so it is human read-able.
print(response['Body'].read().decode('utf-8'))
```

Once you have the response object \(in the aforementioned example it is stored in a variable called `response`\) you can index it to check the HTTP status, the name of the deployed model \(`InvokedProductionVariant`\), and other fields\.

The proceeding code snippet prints the `HTTPStatusCode` stored in the `response` variable:

```
print(response["HTTPStatusCode"])
```

For more information, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html) in the SageMaker API Reference Guide\.

## Test Your Endpoint Using the AWS CLI<a name="realtime-endpoints-test-endpoints-cli"></a>

The following demonstrates how to use `InvokeEndpoint` to check the status of your endpoint using the AWS CLI\. Provide the name of your endpoint\. This is the name you specified for `EndpointName` when you created your endpoint with `CreateEndpoint`\.

Provide input data in the Body field for SageMaker to pass to the model\. The data must be in the same format that was used for training\. The example code template shows how to send binary data to your endpoint\.

```
aws sagemaker-runtime invoke-endpoint \
    --endpoint-name endpoint_name \
    --body fileb://$file_name \
    output_file.txt
```

For more information on when to use `file://` over `fileb://` when passing contents of a file to a parameter of the AWS CLI, see [Best Practices for Local File Parameters](https://aws.amazon.com/blogs/developer/best-practices-for-local-file-parameters/)\.

See `invoke-endpoint` in the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/index.html) guide for more information on additional parameters you can pass\.

If the above invocation shows a snippet such as following, it means the command executed successfully\. Otherwise, check whether input payload is in correct format\.

```
{
    "ContentType": "<content_type>; charset=utf-8",
    "InvokedProductionVariant": "<Variant>"
}
```

View the output of the invocation by checking the file output file \(`output_file.txt` in this example\)\.

```
more output_file.txt
```