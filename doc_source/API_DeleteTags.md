# DeleteTags<a name="API_DeleteTags"></a>

Deletes the specified tags from an Amazon SageMaker resource\.

To list a resource's tags, use the `ListTags` API\. 

**Note**  
When you call this API to delete tags from a hyperparameter tuning job, the deleted tags are not removed from training jobs that the hyperparameter tuning job launched before you called this API\.

## Request Syntax<a name="API_DeleteTags_RequestSyntax"></a>

```
{
   "[ResourceArn](#SageMaker-DeleteTags-request-ResourceArn)": "string",
   "[TagKeys](#SageMaker-DeleteTags-request-TagKeys)": [ "string" ]
}
```

## Request Parameters<a name="API_DeleteTags_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [ResourceArn](#API_DeleteTags_RequestSyntax) **   <a name="SageMaker-DeleteTags-request-ResourceArn"></a>
The Amazon Resource Name \(ARN\) of the resource whose tags you want to delete\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:.*`   
Required: Yes

 ** [TagKeys](#API_DeleteTags_RequestSyntax) **   <a name="SageMaker-DeleteTags-request-TagKeys"></a>
An array or one or more tag keys to delete\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 50 items\.  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^([\p{L}\p{Z}\p{N}_.:/=+\-@]*)$`   
Required: Yes

## Response Elements<a name="API_DeleteTags_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_DeleteTags_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DeleteTags_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DeleteTags) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DeleteTags) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DeleteTags) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DeleteTags) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DeleteTags) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DeleteTags) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DeleteTags) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DeleteTags) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DeleteTags) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DeleteTags) 