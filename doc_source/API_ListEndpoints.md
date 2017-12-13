# ListEndpoints<a name="API_ListEndpoints"></a>

Lists endpoints\.

## Request Syntax<a name="API_ListEndpoints_RequestSyntax"></a>

```
{
   "CreationTimeAfter": number,
   "CreationTimeBefore": number,
   "LastModifiedTimeAfter": number,
   "LastModifiedTimeBefore": number,
   "MaxResults": number,
   "NameContains": "string",
   "NextToken": "string",
   "SortBy": "string",
   "SortOrder": "string",
   "StatusEquals": "string"
}
```

## Request Parameters<a name="API_ListEndpoints_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** CreationTimeAfter **   
A filter that returns only endpoints that were created after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** CreationTimeBefore **   
A filter that returns only endpoints that were created before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** LastModifiedTimeAfter **   
 A filter that returns only endpoints that were modified after the specified timestamp\.   
Type: Timestamp  
Required: No

 ** LastModifiedTimeBefore **   
 A filter that returns only endpoints that were modified before the specified timestamp\.   
Type: Timestamp  
Required: No

 ** MaxResults **   
The maximum number of endpoints to return in the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** NameContains **   
A string in endpoint names\. This filter returns only endpoints whose name contains the specified string\.  
Type: String  
Pattern: `[a-zA-Z0-9-]+`   
Required: No

 ** NextToken **   
If the result of a `ListEndpoints` request was truncated, the response includes a `NextToken`\. To retrieve the next set of endpoints, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** SortBy **   
Sorts the list of results\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreationTime | Status`   
Required: No

 ** SortOrder **   
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** StatusEquals **   
 A filter that returns only endpoints with the specified status\.   
Type: String  
Valid Values:` OutOfService | Creating | Updating | RollingBack | InService | Deleting | Failed`   
Required: No

## Response Syntax<a name="API_ListEndpoints_ResponseSyntax"></a>

```
{
   "Endpoints": [ 
      { 
         "CreationTime": number,
         "EndpointArn": "string",
         "EndpointName": "string",
         "EndpointStatus": "string",
         "LastModifiedTime": number
      }
   ],
   "NextToken": "string"
}
```

## Response Elements<a name="API_ListEndpoints_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** Endpoints **   
 An array or endpoint objects\.   
Type: Array of [EndpointSummary](API_EndpointSummary.md) objects

 ** NextToken **   
 If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of training jobs, use it in the subsequent request\.   
Type: String  
Length Constraints: Maximum length of 8192\.

## Errors<a name="API_ListEndpoints_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListEndpoints_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListEndpoints) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListEndpoints) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListEndpoints) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListEndpoints) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListEndpoints) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListEndpoints) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListEndpoints) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListEndpoints) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListEndpoints) 