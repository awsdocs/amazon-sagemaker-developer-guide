# ListTags<a name="API_ListTags"></a>

Returns the tags for the specified Amazon SageMaker resource\.

## Request Syntax<a name="API_ListTags_RequestSyntax"></a>

```
{
   "MaxResults": number,
   "NextToken": "string",
   "ResourceArn": "string"
}
```

## Request Parameters<a name="API_ListTags_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** MaxResults **   
Maximum number of tags to return\.  
Type: Integer  
Valid Range: Minimum value of 50\.  
Required: No

 ** NextToken **   
 If the response to the previous `ListTags` request is truncated, Amazon SageMaker returns this token\. To retrieve the next set of tags, use it in the subsequent request\.   
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** ResourceArn **   
The Amazon Resource Name \(ARN\) of the resource whose tags you want to retrieve\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

## Response Syntax<a name="API_ListTags_ResponseSyntax"></a>

```
{
   "NextToken": "string",
   "Tags": [ 
      { 
         "Key": "string",
         "Value": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListTags_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** NextToken **   
 If response is truncated, Amazon SageMaker includes a token in the response\. You can use this token in your subsequent request to fetch next set of tokens\.   
Type: String  
Length Constraints: Maximum length of 8192\.

 ** Tags **   
An array of `Tag` objects, each with a tag key and a value\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.

## Errors<a name="API_ListTags_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListTags_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListTags) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListTags) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListTags) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListTags) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListTags) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListTags) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListTags) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListTags) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListTags) 