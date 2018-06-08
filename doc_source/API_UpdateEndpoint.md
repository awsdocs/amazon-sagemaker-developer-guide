# UpdateEndpoint<a name="API_UpdateEndpoint"></a>

 Deploys the new `EndpointConfig` specified in the request, switches to using newly created endpoint, and then deletes resources provisioned for the endpoint using the previous `EndpointConfig` \(there is no availability loss\)\. 

When Amazon SageMaker receives the request, it sets the endpoint status to `Updating`\. After updating the endpoint, it sets the status to `InService`\. To check the status of an endpoint, use the [DescribeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeEndpoint.html) API\. 

## Request Syntax<a name="API_UpdateEndpoint_RequestSyntax"></a>

```
{
   "[EndpointConfigName](#SageMaker-UpdateEndpoint-request-EndpointConfigName)": "string",
   "[EndpointName](#SageMaker-UpdateEndpoint-request-EndpointName)": "string"
}
```

## Request Parameters<a name="API_UpdateEndpoint_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [EndpointConfigName](#API_UpdateEndpoint_RequestSyntax) **   <a name="SageMaker-UpdateEndpoint-request-EndpointConfigName"></a>
The name of the new endpoint configuration\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [EndpointName](#API_UpdateEndpoint_RequestSyntax) **   <a name="SageMaker-UpdateEndpoint-request-EndpointName"></a>
The name of the endpoint whose configuration you want to update\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_UpdateEndpoint_ResponseSyntax"></a>

```
{
   "[EndpointArn](#SageMaker-UpdateEndpoint-response-EndpointArn)": "string"
}
```

## Response Elements<a name="API_UpdateEndpoint_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [EndpointArn](#API_UpdateEndpoint_ResponseSyntax) **   <a name="SageMaker-UpdateEndpoint-response-EndpointArn"></a>
The Amazon Resource Name \(ARN\) of the endpoint\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.

## Errors<a name="API_UpdateEndpoint_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_UpdateEndpoint_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/UpdateEndpoint) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/UpdateEndpoint) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/UpdateEndpoint) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/UpdateEndpoint) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/UpdateEndpoint) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/UpdateEndpoint) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/UpdateEndpoint) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/UpdateEndpoint) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/UpdateEndpoint) 