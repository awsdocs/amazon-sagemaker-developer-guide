# ListModels<a name="API_ListModels"></a>

Lists models created with the [CreateModel](http://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html) API\.

## Request Syntax<a name="API_ListModels_RequestSyntax"></a>

```
{
   "CreationTimeAfter": number,
   "CreationTimeBefore": number,
   "MaxResults": number,
   "NameContains": "string",
   "NextToken": "string",
   "SortBy": "string",
   "SortOrder": "string"
}
```

## Request Parameters<a name="API_ListModels_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** CreationTimeAfter **   
A filter that returns only models created after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** CreationTimeBefore **   
A filter that returns only models created before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** MaxResults **   
The maximum number of models to return in the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** NameContains **   
A string in the training job name\. This filter returns only models in the training job whose name contains the specified string\.  
Type: String  
Pattern: `[a-zA-Z0-9-]+`   
Required: No

 ** NextToken **   
If the response to a previous `ListModels` request was truncated, the response includes a `NextToken`\. To retrieve the next set of models, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** SortBy **   
Sorts the list of results\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreationTime`   
Required: No

 ** SortOrder **   
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

## Response Syntax<a name="API_ListModels_ResponseSyntax"></a>

```
{
   "Models": [ 
      { 
         "CreationTime": number,
         "ModelArn": "string",
         "ModelName": "string"
      }
   ],
   "NextToken": "string"
}
```

## Response Elements<a name="API_ListModels_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** Models **   
An array of `ModelSummary` objects, each of which lists a model\.  
Type: Array of [ModelSummary](API_ModelSummary.md) objects

 ** NextToken **   
 If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of models, use it in the subsequent request\.   
Type: String  
Length Constraints: Maximum length of 8192\.

## Errors<a name="API_ListModels_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListModels_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListModels) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListModels) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListModels) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListModels) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListModels) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListModels) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListModels) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListModels) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListModels) 