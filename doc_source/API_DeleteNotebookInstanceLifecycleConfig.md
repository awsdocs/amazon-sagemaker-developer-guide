# DeleteNotebookInstanceLifecycleConfig<a name="API_DeleteNotebookInstanceLifecycleConfig"></a>

Deletes a notebook instance lifecycle configuration\.

## Request Syntax<a name="API_DeleteNotebookInstanceLifecycleConfig_RequestSyntax"></a>

```
{
   "[NotebookInstanceLifecycleConfigName](#SageMaker-DeleteNotebookInstanceLifecycleConfig-request-NotebookInstanceLifecycleConfigName)": "string"
}
```

## Request Parameters<a name="API_DeleteNotebookInstanceLifecycleConfig_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [NotebookInstanceLifecycleConfigName](#API_DeleteNotebookInstanceLifecycleConfig_RequestSyntax) **   <a name="SageMaker-DeleteNotebookInstanceLifecycleConfig-request-NotebookInstanceLifecycleConfigName"></a>
The name of the lifecycle configuration to delete\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Elements<a name="API_DeleteNotebookInstanceLifecycleConfig_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_DeleteNotebookInstanceLifecycleConfig_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DeleteNotebookInstanceLifecycleConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DeleteNotebookInstanceLifecycleConfig) 
+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DeleteNotebookInstanceLifecycleConfig) 
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DeleteNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DeleteNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DeleteNotebookInstanceLifecycleConfig) 
+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DeleteNotebookInstanceLifecycleConfig) 
+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DeleteNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DeleteNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DeleteNotebookInstanceLifecycleConfig) 