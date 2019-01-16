# CreateNotebookInstanceLifecycleConfig<a name="API_CreateNotebookInstanceLifecycleConfig"></a>

Creates a lifecycle configuration that you can associate with a notebook instance\. A *lifecycle configuration* is a collection of shell scripts that run when you create or start a notebook instance\.

Each lifecycle configuration script has a limit of 16384 characters\.

The value of the `$PATH` environment variable that is available to both scripts is `/sbin:bin:/usr/sbin:/usr/bin`\.

View CloudWatch Logs for notebook instance lifecycle configurations in log group `/aws/sagemaker/NotebookInstances` in log stream `[notebook-instance-name]/[LifecycleConfigHook]`\.

Lifecycle configuration scripts cannot run for longer than 5 minutes\. If a script runs for longer than 5 minutes, it fails and the notebook instance is not created or started\.

For information about notebook instance lifestyle configurations, see [Step 2\.1: \(Optional\) Customize a Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/notebook-lifecycle-config.html)\.

## Request Syntax<a name="API_CreateNotebookInstanceLifecycleConfig_RequestSyntax"></a>

```
{
   "[NotebookInstanceLifecycleConfigName](#SageMaker-CreateNotebookInstanceLifecycleConfig-request-NotebookInstanceLifecycleConfigName)": "string",
   "[OnCreate](#SageMaker-CreateNotebookInstanceLifecycleConfig-request-OnCreate)": [ 
      { 
         "[Content](API_NotebookInstanceLifecycleHook.md#SageMaker-Type-NotebookInstanceLifecycleHook-Content)": "string"
      }
   ],
   "[OnStart](#SageMaker-CreateNotebookInstanceLifecycleConfig-request-OnStart)": [ 
      { 
         "[Content](API_NotebookInstanceLifecycleHook.md#SageMaker-Type-NotebookInstanceLifecycleHook-Content)": "string"
      }
   ]
}
```

## Request Parameters<a name="API_CreateNotebookInstanceLifecycleConfig_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [NotebookInstanceLifecycleConfigName](#API_CreateNotebookInstanceLifecycleConfig_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstanceLifecycleConfig-request-NotebookInstanceLifecycleConfigName"></a>
The name of the lifecycle configuration\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [OnCreate](#API_CreateNotebookInstanceLifecycleConfig_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstanceLifecycleConfig-request-OnCreate"></a>
A shell script that runs only once, when you create a notebook instance\. The shell script must be a base64\-encoded string\.  
Type: Array of [NotebookInstanceLifecycleHook](API_NotebookInstanceLifecycleHook.md) objects  
Array Members: Maximum number of 1 item\.  
Required: No

 ** [OnStart](#API_CreateNotebookInstanceLifecycleConfig_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstanceLifecycleConfig-request-OnStart"></a>
A shell script that runs every time you start a notebook instance, including when you create the notebook instance\. The shell script must be a base64\-encoded string\.  
Type: Array of [NotebookInstanceLifecycleHook](API_NotebookInstanceLifecycleHook.md) objects  
Array Members: Maximum number of 1 item\.  
Required: No

## Response Syntax<a name="API_CreateNotebookInstanceLifecycleConfig_ResponseSyntax"></a>

```
{
   "[NotebookInstanceLifecycleConfigArn](#SageMaker-CreateNotebookInstanceLifecycleConfig-response-NotebookInstanceLifecycleConfigArn)": "string"
}
```

## Response Elements<a name="API_CreateNotebookInstanceLifecycleConfig_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NotebookInstanceLifecycleConfigArn](#API_CreateNotebookInstanceLifecycleConfig_ResponseSyntax) **   <a name="SageMaker-CreateNotebookInstanceLifecycleConfig-response-NotebookInstanceLifecycleConfigArn"></a>
The Amazon Resource Name \(ARN\) of the lifecycle configuration\.  
Type: String  
Length Constraints: Maximum length of 256\.

## Errors<a name="API_CreateNotebookInstanceLifecycleConfig_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateNotebookInstanceLifecycleConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateNotebookInstanceLifecycleConfig) 