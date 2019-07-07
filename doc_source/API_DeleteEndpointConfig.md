# DeleteEndpointConfig<a name="API_DeleteEndpointConfig"></a>

Deletes an endpoint configuration\. The `DeleteEndpointConfig` API deletes only the specified configuration\. It does not delete endpoints created using the configuration\. 

## Request Syntax<a name="API_DeleteEndpointConfig_RequestSyntax"></a>

```
{
   "[EndpointConfigName](#SageMaker-DeleteEndpointConfig-request-EndpointConfigName)": "string"
}
```

## Request Parameters<a name="API_DeleteEndpointConfig_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [EndpointConfigName](#API_DeleteEndpointConfig_RequestSyntax) **   <a name="SageMaker-DeleteEndpointConfig-request-EndpointConfigName"></a>
The name of the endpoint configuration that you want to delete\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Elements<a name="API_DeleteEndpointConfig_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_DeleteEndpointConfig_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DeleteEndpointConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DeleteEndpointConfig) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DeleteEndpointConfig) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DeleteEndpointConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DeleteEndpointConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DeleteEndpointConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DeleteEndpointConfig) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DeleteEndpointConfig) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DeleteEndpointConfig) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DeleteEndpointConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DeleteEndpointConfig) 