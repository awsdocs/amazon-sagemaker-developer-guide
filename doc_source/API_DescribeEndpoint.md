# DescribeEndpoint<a name="API_DescribeEndpoint"></a>

Returns the description of an endpoint\.

## Request Syntax<a name="API_DescribeEndpoint_RequestSyntax"></a>

```
{
   "[EndpointName](#SageMaker-DescribeEndpoint-request-EndpointName)": "string"
}
```

## Request Parameters<a name="API_DescribeEndpoint_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [EndpointName](#API_DescribeEndpoint_RequestSyntax) **   <a name="SageMaker-DescribeEndpoint-request-EndpointName"></a>
The name of the endpoint\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeEndpoint_ResponseSyntax"></a>

```
{
   "[CreationTime](#SageMaker-DescribeEndpoint-response-CreationTime)": number,
   "[EndpointArn](#SageMaker-DescribeEndpoint-response-EndpointArn)": "string",
   "[EndpointConfigName](#SageMaker-DescribeEndpoint-response-EndpointConfigName)": "string",
   "[EndpointName](#SageMaker-DescribeEndpoint-response-EndpointName)": "string",
   "[EndpointStatus](#SageMaker-DescribeEndpoint-response-EndpointStatus)": "string",
   "[FailureReason](#SageMaker-DescribeEndpoint-response-FailureReason)": "string",
   "[LastModifiedTime](#SageMaker-DescribeEndpoint-response-LastModifiedTime)": number,
   "[ProductionVariants](#SageMaker-DescribeEndpoint-response-ProductionVariants)": [ 
      { 
         "[CurrentInstanceCount](API_ProductionVariantSummary.md#SageMaker-Type-ProductionVariantSummary-CurrentInstanceCount)": number,
         "[CurrentWeight](API_ProductionVariantSummary.md#SageMaker-Type-ProductionVariantSummary-CurrentWeight)": number,
         "[DesiredInstanceCount](API_ProductionVariantSummary.md#SageMaker-Type-ProductionVariantSummary-DesiredInstanceCount)": number,
         "[DesiredWeight](API_ProductionVariantSummary.md#SageMaker-Type-ProductionVariantSummary-DesiredWeight)": number,
         "[VariantName](API_ProductionVariantSummary.md#SageMaker-Type-ProductionVariantSummary-VariantName)": "string"
      }
   ]
}
```

## Response Elements<a name="API_DescribeEndpoint_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CreationTime](#API_DescribeEndpoint_ResponseSyntax) **   <a name="SageMaker-DescribeEndpoint-response-CreationTime"></a>
A timestamp that shows when the endpoint was created\.  
Type: Timestamp

 ** [EndpointArn](#API_DescribeEndpoint_ResponseSyntax) **   <a name="SageMaker-DescribeEndpoint-response-EndpointArn"></a>
The Amazon Resource Name \(ARN\) of the endpoint\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.

 ** [EndpointConfigName](#API_DescribeEndpoint_ResponseSyntax) **   <a name="SageMaker-DescribeEndpoint-response-EndpointConfigName"></a>
The name of the endpoint configuration associated with this endpoint\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [EndpointName](#API_DescribeEndpoint_ResponseSyntax) **   <a name="SageMaker-DescribeEndpoint-response-EndpointName"></a>
Name of the endpoint\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [EndpointStatus](#API_DescribeEndpoint_ResponseSyntax) **   <a name="SageMaker-DescribeEndpoint-response-EndpointStatus"></a>
The status of the endpoint\.  
Type: String  
Valid Values:` OutOfService | Creating | Updating | RollingBack | InService | Deleting | Failed` 

 ** [FailureReason](#API_DescribeEndpoint_ResponseSyntax) **   <a name="SageMaker-DescribeEndpoint-response-FailureReason"></a>
If the status of the endpoint is `Failed`, the reason why it failed\.   
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [LastModifiedTime](#API_DescribeEndpoint_ResponseSyntax) **   <a name="SageMaker-DescribeEndpoint-response-LastModifiedTime"></a>
A timestamp that shows when the endpoint was last modified\.  
Type: Timestamp

 ** [ProductionVariants](#API_DescribeEndpoint_ResponseSyntax) **   <a name="SageMaker-DescribeEndpoint-response-ProductionVariants"></a>
 An array of ProductionVariant objects, one for each model hosted behind this endpoint\.   
Type: Array of [ProductionVariantSummary](API_ProductionVariantSummary.md) objects  
Array Members: Minimum number of 1 item\.

## Errors<a name="API_DescribeEndpoint_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeEndpoint_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeEndpoint) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeEndpoint) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeEndpoint) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeEndpoint) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeEndpoint) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeEndpoint) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeEndpoint) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeEndpoint) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeEndpoint) 