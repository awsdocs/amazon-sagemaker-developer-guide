# ListTags<a name="API_ListTags"></a>

Returns the tags for the specified Amazon SageMaker resource\.

## Request Syntax<a name="API_ListTags_RequestSyntax"></a>

```
{
   "[MaxResults](#SageMaker-ListTags-request-MaxResults)": number,
   "[NextToken](#SageMaker-ListTags-request-NextToken)": "string",
   "[ResourceArn](#SageMaker-ListTags-request-ResourceArn)": "string"
}
```

## Request Parameters<a name="API_ListTags_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [MaxResults](#API_ListTags_RequestSyntax) **   <a name="SageMaker-ListTags-request-MaxResults"></a>
Maximum number of tags to return\.  
Type: Integer  
Valid Range: Minimum value of 50\.  
Required: No

 ** [NextToken](#API_ListTags_RequestSyntax) **   <a name="SageMaker-ListTags-request-NextToken"></a>
 If the response to the previous `ListTags` request is truncated, Amazon SageMaker returns this token\. To retrieve the next set of tags, use it in the subsequent request\.   
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [ResourceArn](#API_ListTags_RequestSyntax) **   <a name="SageMaker-ListTags-request-ResourceArn"></a>
The Amazon Resource Name \(ARN\) of the resource whose tags you want to retrieve\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

## Response Syntax<a name="API_ListTags_ResponseSyntax"></a>

```
{
   "[NextToken](#SageMaker-ListTags-response-NextToken)": "string",
   "[Tags](#SageMaker-ListTags-response-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListTags_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NextToken](#API_ListTags_ResponseSyntax) **   <a name="SageMaker-ListTags-response-NextToken"></a>
 If response is truncated, Amazon SageMaker includes a token in the response\. You can use this token in your subsequent request to fetch next set of tokens\.   
Type: String  
Length Constraints: Maximum length of 8192\.

 ** [Tags](#API_ListTags_ResponseSyntax) **   <a name="SageMaker-ListTags-response-Tags"></a>
An array of `Tag` objects, each with a tag key and a value\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.

## Errors<a name="API_ListTags_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListTags_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListTags) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListTags) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListTags) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListTags) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListTags) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListTags) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListTags) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListTags) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListTags) 