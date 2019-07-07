# DeleteWorkteam<a name="API_DeleteWorkteam"></a>

Deletes an existing work team\. This operation can't be undone\.

## Request Syntax<a name="API_DeleteWorkteam_RequestSyntax"></a>

```
{
   "[WorkteamName](#SageMaker-DeleteWorkteam-request-WorkteamName)": "string"
}
```

## Request Parameters<a name="API_DeleteWorkteam_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [WorkteamName](#API_DeleteWorkteam_RequestSyntax) **   <a name="SageMaker-DeleteWorkteam-request-WorkteamName"></a>
The name of the work team to delete\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DeleteWorkteam_ResponseSyntax"></a>

```
{
   "[Success](#SageMaker-DeleteWorkteam-response-Success)": boolean
}
```

## Response Elements<a name="API_DeleteWorkteam_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Success](#API_DeleteWorkteam_ResponseSyntax) **   <a name="SageMaker-DeleteWorkteam-response-Success"></a>
Returns `true` if the work team was successfully deleted; otherwise, returns `false`\.  
Type: Boolean

## Errors<a name="API_DeleteWorkteam_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_DeleteWorkteam_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DeleteWorkteam) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DeleteWorkteam) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DeleteWorkteam) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DeleteWorkteam) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DeleteWorkteam) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DeleteWorkteam) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DeleteWorkteam) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DeleteWorkteam) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DeleteWorkteam) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DeleteWorkteam) 