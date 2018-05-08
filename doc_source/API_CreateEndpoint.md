# CreateEndpoint<a name="API_CreateEndpoint"></a>

Creates an endpoint using the endpoint configuration specified in the request\. Amazon SageMaker uses the endpoint to provision resources and deploy models\. You create the endpoint configuration with the [CreateEndpointConfig](http://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html) API\. 

**Note**  
 Use this API only for hosting models using Amazon SageMaker hosting services\. 

The endpoint name must be unique within an AWS Region in your AWS account\. 

When it receives the request, Amazon SageMaker creates the endpoint, launches the resources \(ML compute instances\), and deploys the model\(s\) on them\. 

When Amazon SageMaker receives the request, it sets the endpoint status to `Creating`\. After it creates the endpoint, it sets the status to `InService`\. Amazon SageMaker can then process incoming requests for inferences\. To check the status of an endpoint, use the [DescribeEndpoint](http://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeEndpoint.html) API\.

For an example, see [Exercise 1: Using the K\-Means Algorithm Provided by Amazon SageMaker](http://docs.aws.amazon.com/sagemaker/latest/dg/ex1.html)\. 

## Request Syntax<a name="API_CreateEndpoint_RequestSyntax"></a>

```
{
   "[EndpointConfigName](#SageMaker-CreateEndpoint-request-EndpointConfigName)": "string",
   "[EndpointName](#SageMaker-CreateEndpoint-request-EndpointName)": "string",
   "[Tags](#SageMaker-CreateEndpoint-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ]
}
```

## Request Parameters<a name="API_CreateEndpoint_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [EndpointConfigName](#API_CreateEndpoint_RequestSyntax) **   <a name="SageMaker-CreateEndpoint-request-EndpointConfigName"></a>
The name of an endpoint configuration\. For more information, see [CreateEndpointConfig](http://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html)\.   
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [EndpointName](#API_CreateEndpoint_RequestSyntax) **   <a name="SageMaker-CreateEndpoint-request-EndpointName"></a>
The name of the endpoint\. The name must be unique within an AWS Region in your AWS account\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [Tags](#API_CreateEndpoint_RequestSyntax) **   <a name="SageMaker-CreateEndpoint-request-Tags"></a>
An array of key\-value pairs\. For more information, see [Using Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what)in the *AWS Billing and Cost Management User Guide*\.   
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

## Response Syntax<a name="API_CreateEndpoint_ResponseSyntax"></a>

```
{
   "[EndpointArn](#SageMaker-CreateEndpoint-response-EndpointArn)": "string"
}
```

## Response Elements<a name="API_CreateEndpoint_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [EndpointArn](#API_CreateEndpoint_ResponseSyntax) **   <a name="SageMaker-CreateEndpoint-response-EndpointArn"></a>
The Amazon Resource Name \(ARN\) of the endpoint\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.

## Errors<a name="API_CreateEndpoint_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateEndpoint_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateEndpoint) 
+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateEndpoint) 
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateEndpoint) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateEndpoint) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateEndpoint) 
+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateEndpoint) 
+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateEndpoint) 
+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateEndpoint) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateEndpoint) 