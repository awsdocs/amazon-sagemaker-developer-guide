# Invoke an Asynchronous Endpoint<a name="async-inference-invoke-endpoint"></a>

Get inferences from the model hosted at your asynchronous endpoint with `InvokeEndpointAsync`\. 

**Note**  
If you have not done so already, upload your inference data \(e\.g\., machine learning model, sample data\) to Amazon S3\.

Specify the following fields in your request:
+ For `InputLocation`, specify the location of your inference data\.
+ For `EndpointName`, specify the name of your endpoint\.
+ \(Optional\) For `InvocationTimeoutSeconds`, you can set the max timeout for the requests\. You can set this value to a maximum of 3600 seconds \(one hour\) on a per\-request basis\. If you don't specify this field in your request, by default the request times out at 15 minutes\.

```
# Create a low-level client representing Amazon SageMaker Runtime
sagemaker_runtime = boto3.client("sagemaker-runtime", region_name=<aws_region>)

# Specify the location of the input. Here, a single SVM sample
input_location = "s3://bucket-name/test_point_0.libsvm"

# The name of the endpoint. The name must be unique within an AWS Region in your AWS account. 
endpoint_name='<endpoint-name>'

# After you deploy a model into production using SageMaker hosting 
# services, your client applications use this API to get inferences 
# from the model hosted at the specified endpoint.
response = sagemaker_runtime.invoke_endpoint_async(
                            EndpointName=endpoint_name, 
                            InputLocation=input_location,
                            InvocationTimeoutSeconds=3600)
```

You receive a response as a JSON string with your request ID and the name of the Amazon S3 bucket that will have the response to the API call after it is processed\.