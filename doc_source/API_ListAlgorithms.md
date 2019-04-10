# ListAlgorithms<a name="API_ListAlgorithms"></a>

Lists the machine learning algorithms that have been created\.

## Request Syntax<a name="API_ListAlgorithms_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListAlgorithms-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListAlgorithms-request-CreationTimeBefore)": number,
   "[MaxResults](#SageMaker-ListAlgorithms-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListAlgorithms-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListAlgorithms-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListAlgorithms-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListAlgorithms-request-SortOrder)": "string"
}
```

## Request Parameters<a name="API_ListAlgorithms_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListAlgorithms_RequestSyntax) **   <a name="SageMaker-ListAlgorithms-request-CreationTimeAfter"></a>
A filter that returns only algorithms created after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListAlgorithms_RequestSyntax) **   <a name="SageMaker-ListAlgorithms-request-CreationTimeBefore"></a>
A filter that returns only algorithms created before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListAlgorithms_RequestSyntax) **   <a name="SageMaker-ListAlgorithms-request-MaxResults"></a>
The maximum number of algorithms to return in the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListAlgorithms_RequestSyntax) **   <a name="SageMaker-ListAlgorithms-request-NameContains"></a>
A string in the algorithm name\. This filter returns only algorithms whose name contains the specified string\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9\-]+`   
Required: No

 ** [NextToken](#API_ListAlgorithms_RequestSyntax) **   <a name="SageMaker-ListAlgorithms-request-NextToken"></a>
If the response to a previous `ListAlgorithms` request was truncated, the response includes a `NextToken`\. To retrieve the next set of algorithms, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*`   
Required: No

 ** [SortBy](#API_ListAlgorithms_RequestSyntax) **   <a name="SageMaker-ListAlgorithms-request-SortBy"></a>
The parameter by which to sort the results\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreationTime`   
Required: No

 ** [SortOrder](#API_ListAlgorithms_RequestSyntax) **   <a name="SageMaker-ListAlgorithms-request-SortOrder"></a>
The sort order for the results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

## Response Syntax<a name="API_ListAlgorithms_ResponseSyntax"></a>

```
{
   "[AlgorithmSummaryList](#SageMaker-ListAlgorithms-response-AlgorithmSummaryList)": [ 
      { 
         "[AlgorithmArn](API_AlgorithmSummary.md#SageMaker-Type-AlgorithmSummary-AlgorithmArn)": "string",
         "[AlgorithmDescription](API_AlgorithmSummary.md#SageMaker-Type-AlgorithmSummary-AlgorithmDescription)": "string",
         "[AlgorithmName](API_AlgorithmSummary.md#SageMaker-Type-AlgorithmSummary-AlgorithmName)": "string",
         "[AlgorithmStatus](API_AlgorithmSummary.md#SageMaker-Type-AlgorithmSummary-AlgorithmStatus)": "string",
         "[CreationTime](API_AlgorithmSummary.md#SageMaker-Type-AlgorithmSummary-CreationTime)": number
      }
   ],
   "[NextToken](#SageMaker-ListAlgorithms-response-NextToken)": "string"
}
```

## Response Elements<a name="API_ListAlgorithms_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AlgorithmSummaryList](#API_ListAlgorithms_ResponseSyntax) **   <a name="SageMaker-ListAlgorithms-response-AlgorithmSummaryList"></a>
>An array of `AlgorithmSummary` objects, each of which lists an algorithm\.  
Type: Array of [AlgorithmSummary](API_AlgorithmSummary.md) objects

 ** [NextToken](#API_ListAlgorithms_ResponseSyntax) **   <a name="SageMaker-ListAlgorithms-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of algorithms, use it in the subsequent request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*` 

## Errors<a name="API_ListAlgorithms_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListAlgorithms_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListAlgorithms) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListAlgorithms) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListAlgorithms) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListAlgorithms) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ListAlgorithms) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListAlgorithms) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListAlgorithms) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListAlgorithms) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListAlgorithms) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListAlgorithms) 