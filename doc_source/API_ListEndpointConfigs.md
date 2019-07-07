# ListEndpointConfigs<a name="API_ListEndpointConfigs"></a>

Lists endpoint configurations\.

## Request Syntax<a name="API_ListEndpointConfigs_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListEndpointConfigs-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListEndpointConfigs-request-CreationTimeBefore)": number,
   "[MaxResults](#SageMaker-ListEndpointConfigs-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListEndpointConfigs-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListEndpointConfigs-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListEndpointConfigs-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListEndpointConfigs-request-SortOrder)": "string"
}
```

## Request Parameters<a name="API_ListEndpointConfigs_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListEndpointConfigs_RequestSyntax) **   <a name="SageMaker-ListEndpointConfigs-request-CreationTimeAfter"></a>
A filter that returns only endpoint configurations with a creation time greater than or equal to the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListEndpointConfigs_RequestSyntax) **   <a name="SageMaker-ListEndpointConfigs-request-CreationTimeBefore"></a>
A filter that returns only endpoint configurations created before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListEndpointConfigs_RequestSyntax) **   <a name="SageMaker-ListEndpointConfigs-request-MaxResults"></a>
The maximum number of training jobs to return in the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListEndpointConfigs_RequestSyntax) **   <a name="SageMaker-ListEndpointConfigs-request-NameContains"></a>
A string in the endpoint configuration name\. This filter returns only endpoint configurations whose name contains the specified string\.   
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9-]+`   
Required: No

 ** [NextToken](#API_ListEndpointConfigs_RequestSyntax) **   <a name="SageMaker-ListEndpointConfigs-request-NextToken"></a>
If the result of the previous `ListEndpointConfig` request was truncated, the response includes a `NextToken`\. To retrieve the next set of endpoint configurations, use the token in the next request\.   
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*`   
Required: No

 ** [SortBy](#API_ListEndpointConfigs_RequestSyntax) **   <a name="SageMaker-ListEndpointConfigs-request-SortBy"></a>
The field to sort results by\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreationTime`   
Required: No

 ** [SortOrder](#API_ListEndpointConfigs_RequestSyntax) **   <a name="SageMaker-ListEndpointConfigs-request-SortOrder"></a>
The sort order for results\. The default is `Descending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

## Response Syntax<a name="API_ListEndpointConfigs_ResponseSyntax"></a>

```
{
   "[EndpointConfigs](#SageMaker-ListEndpointConfigs-response-EndpointConfigs)": [ 
      { 
         "[CreationTime](API_EndpointConfigSummary.md#SageMaker-Type-EndpointConfigSummary-CreationTime)": number,
         "[EndpointConfigArn](API_EndpointConfigSummary.md#SageMaker-Type-EndpointConfigSummary-EndpointConfigArn)": "string",
         "[EndpointConfigName](API_EndpointConfigSummary.md#SageMaker-Type-EndpointConfigSummary-EndpointConfigName)": "string"
      }
   ],
   "[NextToken](#SageMaker-ListEndpointConfigs-response-NextToken)": "string"
}
```

## Response Elements<a name="API_ListEndpointConfigs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [EndpointConfigs](#API_ListEndpointConfigs_ResponseSyntax) **   <a name="SageMaker-ListEndpointConfigs-response-EndpointConfigs"></a>
An array of endpoint configurations\.  
Type: Array of [EndpointConfigSummary](API_EndpointConfigSummary.md) objects

 ** [NextToken](#API_ListEndpointConfigs_ResponseSyntax) **   <a name="SageMaker-ListEndpointConfigs-response-NextToken"></a>
 If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of endpoint configurations, use it in the subsequent request   
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*` 

## Errors<a name="API_ListEndpointConfigs_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListEndpointConfigs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListEndpointConfigs) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListEndpointConfigs) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListEndpointConfigs) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListEndpointConfigs) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ListEndpointConfigs) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListEndpointConfigs) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListEndpointConfigs) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListEndpointConfigs) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListEndpointConfigs) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListEndpointConfigs) 