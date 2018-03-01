# AddTags<a name="API_AddTags"></a>

Adds or overwrites one or more tags for the specified Amazon SageMaker resource\. You can add tags to notebook instances, training jobs, models, endpoint configurations, and endpoints\. 

Each tag consists of a key and an optional value\. Tag keys must be unique per resource\. For more information about tags, see [Using Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\. 

## Request Syntax<a name="API_AddTags_RequestSyntax"></a>

```
{
   "[ResourceArn](#SageMaker-AddTags-request-ResourceArn)": "string",
   "[Tags](#SageMaker-AddTags-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ]
}
```

## Request Parameters<a name="API_AddTags_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [ResourceArn](#API_AddTags_RequestSyntax) **   <a name="SageMaker-AddTags-request-ResourceArn"></a>
The Amazon Resource Name \(ARN\) of the resource that you want to tag\.   
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

 ** [Tags](#API_AddTags_RequestSyntax) **   <a name="SageMaker-AddTags-request-Tags"></a>
An array of `Tag` objects\. Each tag is a key\-value pair\. Only the `key` parameter is required\. If you don't specify a value, Amazon SageMaker sets the value to an empty string\.   
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: Yes

## Response Syntax<a name="API_AddTags_ResponseSyntax"></a>

```
{
   "[Tags](#SageMaker-AddTags-response-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ]
}
```

## Response Elements<a name="API_AddTags_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Tags](#API_AddTags_ResponseSyntax) **   <a name="SageMaker-AddTags-response-Tags"></a>
A list of tags associated with the Amazon SageMaker resource\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.

## Errors<a name="API_AddTags_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_AddTags_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/AddTags) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/AddTags) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/AddTags) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/AddTags) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/AddTags) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/AddTags) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/AddTags) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/AddTags) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/AddTags) 