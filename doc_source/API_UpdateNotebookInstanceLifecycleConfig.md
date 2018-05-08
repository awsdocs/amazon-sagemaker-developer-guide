# UpdateNotebookInstanceLifecycleConfig<a name="API_UpdateNotebookInstanceLifecycleConfig"></a>

Updates a notebook instance lifecycle configuration created with the [CreateNotebookInstanceLifecycleConfig](API_CreateNotebookInstanceLifecycleConfig.md) API\.

## Request Syntax<a name="API_UpdateNotebookInstanceLifecycleConfig_RequestSyntax"></a>

```
{
   "[NotebookInstanceLifecycleConfigName](#SageMaker-UpdateNotebookInstanceLifecycleConfig-request-NotebookInstanceLifecycleConfigName)": "string",
   "[OnCreate](#SageMaker-UpdateNotebookInstanceLifecycleConfig-request-OnCreate)": [ 
      { 
         "[Content](API_NotebookInstanceLifecycleHook.md#SageMaker-Type-NotebookInstanceLifecycleHook-Content)": "string"
      }
   ],
   "[OnStart](#SageMaker-UpdateNotebookInstanceLifecycleConfig-request-OnStart)": [ 
      { 
         "[Content](API_NotebookInstanceLifecycleHook.md#SageMaker-Type-NotebookInstanceLifecycleHook-Content)": "string"
      }
   ]
}
```

## Request Parameters<a name="API_UpdateNotebookInstanceLifecycleConfig_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [NotebookInstanceLifecycleConfigName](#API_UpdateNotebookInstanceLifecycleConfig_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstanceLifecycleConfig-request-NotebookInstanceLifecycleConfigName"></a>
The name of the lifecycle configuration\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [OnCreate](#API_UpdateNotebookInstanceLifecycleConfig_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstanceLifecycleConfig-request-OnCreate"></a>
The shell script that runs only once, when you create a notebook instance  
Type: Array of [NotebookInstanceLifecycleHook](API_NotebookInstanceLifecycleHook.md) objects  
Array Members: Maximum number of 1 item\.  
Required: No

 ** [OnStart](#API_UpdateNotebookInstanceLifecycleConfig_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstanceLifecycleConfig-request-OnStart"></a>
The shell script that runs every time you start a notebook instance, including when you create the notebook instance\.  
Type: Array of [NotebookInstanceLifecycleHook](API_NotebookInstanceLifecycleHook.md) objects  
Array Members: Maximum number of 1 item\.  
Required: No

## Response Elements<a name="API_UpdateNotebookInstanceLifecycleConfig_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_UpdateNotebookInstanceLifecycleConfig_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_UpdateNotebookInstanceLifecycleConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/UpdateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/UpdateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/UpdateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/UpdateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/UpdateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/UpdateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/UpdateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/UpdateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/UpdateNotebookInstanceLifecycleConfig) 