# InvokeEndpoint<a name="API_runtime_InvokeEndpoint"></a>

After you deploy a model into production using Amazon SageMaker hosting services, your client applications use this API to get inferences from the model hosted at the specified endpoint\. 

For an overview of Amazon SageMaker, see [How It Works](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works.html)\. 

Amazon SageMaker strips all POST headers except those supported by the API\. Amazon SageMaker might add additional headers\. You should not rely on the behavior of headers outside those enumerated in the request syntax\. 

Calls to `InvokeEndpoint` are authenticated by using AWS Signature Version 4\. For information, see [Authenticating Requests \(AWS Signature Version 4\)](http://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html) in the *Amazon S3 API Reference*\.

**Note**  
Endpoints are scoped to an individual account, and are not public\. The URL does not contain the account ID, but Amazon SageMaker determines the account ID from the authentication token that is supplied by the caller\.

## Request Syntax<a name="API_runtime_InvokeEndpoint_RequestSyntax"></a>

```
POST /endpoints/EndpointName/invocations HTTP/1.1
Content-Type: ContentType
Accept: Accept

Body
```

## URI Request Parameters<a name="API_runtime_InvokeEndpoint_RequestParameters"></a>

The request requires the following URI parameters\.

 ** [Accept](#API_runtime_InvokeEndpoint_RequestSyntax) **   <a name="SageMaker-runtime_InvokeEndpoint-request-Accept"></a>
The desired MIME type of the inference in the response\.  
Length Constraints: Maximum length of 1024\.

 ** [ContentType](#API_runtime_InvokeEndpoint_RequestSyntax) **   <a name="SageMaker-runtime_InvokeEndpoint-request-ContentType"></a>
The MIME type of the input data in the request body\.  
Length Constraints: Maximum length of 1024\.

 ** [EndpointName](#API_runtime_InvokeEndpoint_RequestSyntax) **   <a name="SageMaker-runtime_InvokeEndpoint-request-EndpointName"></a>
The name of the endpoint that you specified when you created the endpoint using the [CreateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html) API\.   
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

## Request Body<a name="API_runtime_InvokeEndpoint_RequestBody"></a>

The request accepts the following binary data\.

 ** [Body](#API_runtime_InvokeEndpoint_RequestSyntax) **   <a name="SageMaker-runtime_InvokeEndpoint-request-Body"></a>
Provides input data, in the format specified in the `ContentType` request header\. Amazon SageMaker passes all of the data in the body to the model\.   
For information about the format of the request body, see [Common Data Formats—Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html)\.  
Length Constraints: Maximum length of 5242880\.

## Response Syntax<a name="API_runtime_InvokeEndpoint_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-Type: ContentType
x-Amzn-Invoked-Production-Variant: InvokedProductionVariant

Body
```

## Response Elements<a name="API_runtime_InvokeEndpoint_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The response returns the following HTTP headers\.

 ** [ContentType](#API_runtime_InvokeEndpoint_ResponseSyntax) **   <a name="SageMaker-runtime_InvokeEndpoint-response-ContentType"></a>
The MIME type of the inference returned in the response body\.  
Length Constraints: Maximum length of 1024\.

 ** [InvokedProductionVariant](#API_runtime_InvokeEndpoint_ResponseSyntax) **   <a name="SageMaker-runtime_InvokeEndpoint-response-InvokedProductionVariant"></a>
Identifies the production variant that was invoked\.  
Length Constraints: Maximum length of 1024\.

The response returns the following as the HTTP body\.

 ** [Body](#API_runtime_InvokeEndpoint_ResponseSyntax) **   <a name="SageMaker-runtime_InvokeEndpoint-response-Body"></a>
Includes the inference provided by the model\.  
For information about the format of the response body, see [Common Data Formats—Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html)\.  
Length Constraints: Maximum length of 5242880\.

## Errors<a name="API_runtime_InvokeEndpoint_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **InternalFailure**   
 An internal failure occurred\.   
HTTP Status Code: 500

 **ModelError**   
 Model \(owned by the customer in the container\) returned an error 500\.   
HTTP Status Code: 424

 **ServiceUnavailable**   
 The service is unavailable\. Try your call again\.   
HTTP Status Code: 503

 **ValidationError**   
 Inspect your request and try again\.   
HTTP Status Code: 400

## See Also<a name="API_runtime_InvokeEndpoint_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/runtime.sagemaker-2017-05-13/InvokeEndpoint) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/runtime.sagemaker-2017-05-13/InvokeEndpoint) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/runtime.sagemaker-2017-05-13/InvokeEndpoint) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/runtime.sagemaker-2017-05-13/InvokeEndpoint) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/runtime.sagemaker-2017-05-13/InvokeEndpoint) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/runtime.sagemaker-2017-05-13/InvokeEndpoint) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/runtime.sagemaker-2017-05-13/InvokeEndpoint) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/runtime.sagemaker-2017-05-13/InvokeEndpoint) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/runtime.sagemaker-2017-05-13/InvokeEndpoint) 
