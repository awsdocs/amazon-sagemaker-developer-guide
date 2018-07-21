# DescribeNotebookInstanceLifecycleConfig<a name="API_DescribeNotebookInstanceLifecycleConfig"></a>

Returns a description of a notebook instance lifecycle configuration\.

For information about notebook instance lifestyle configurations, see [Step 2\.1: \(Optional\) Customize a Notebook Instance ](notebook-lifecycle-config.md)\.

## Response Syntax<a name="API_DescribeNotebookInstanceLifecycleConfig_ResponseSyntax"></a>

```
{
   "[CreationTime](#SageMaker-DescribeNotebookInstanceLifecycleConfig-response-CreationTime)": number,
   "[LastModifiedTime](#SageMaker-DescribeNotebookInstanceLifecycleConfig-response-LastModifiedTime)": number,
   "[NotebookInstanceLifecycleConfigArn](#SageMaker-DescribeNotebookInstanceLifecycleConfig-response-NotebookInstanceLifecycleConfigArn)": "string",
   "[OnCreate](#SageMaker-DescribeNotebookInstanceLifecycleConfig-response-OnCreate)": [ 
      { 
         "[Content](API_NotebookInstanceLifecycleHook.md#SageMaker-Type-NotebookInstanceLifecycleHook-Content)": "string"
      }
   ],
   "[OnStart](#SageMaker-DescribeNotebookInstanceLifecycleConfig-response-OnStart)": [ 
      { 
         "[Content](API_NotebookInstanceLifecycleHook.md#SageMaker-Type-NotebookInstanceLifecycleHook-Content)": "string"
      }
   ]
}
```

## Response Elements<a name="API_DescribeNotebookInstanceLifecycleConfig_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CreationTime](#API_DescribeNotebookInstanceLifecycleConfig_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstanceLifecycleConfig-response-CreationTime"></a>
A timestamp that tells when the lifecycle configuration was created\.  
Type: Timestamp

 ** [LastModifiedTime](#API_DescribeNotebookInstanceLifecycleConfig_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstanceLifecycleConfig-response-LastModifiedTime"></a>
A timestamp that tells when the lifecycle configuration was last modified\.  
Type: Timestamp

 ** [NotebookInstanceLifecycleConfigArn](#API_DescribeNotebookInstanceLifecycleConfig_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstanceLifecycleConfig-response-NotebookInstanceLifecycleConfigArn"></a>
The Amazon Resource Name \(ARN\) of the lifecycle configuration\.  
Type: String  
Length Constraints: Maximum length of 256\.

 ** [OnCreate](#API_DescribeNotebookInstanceLifecycleConfig_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstanceLifecycleConfig-response-OnCreate"></a>
The shell script that runs only once, when you create a notebook instance\.  
Type: Array of [NotebookInstanceLifecycleHook](API_NotebookInstanceLifecycleHook.md) objects  
Array Members: Maximum number of 1 item\.

 ** [OnStart](#API_DescribeNotebookInstanceLifecycleConfig_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstanceLifecycleConfig-response-OnStart"></a>
The shell script that runs every time you start a notebook instance, including when you create the notebook instance\.  
Type: Array of [NotebookInstanceLifecycleHook](API_NotebookInstanceLifecycleHook.md) objects  
Array Members: Maximum number of 1 item\.

## Errors<a name="API_DescribeNotebookInstanceLifecycleConfig_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeNotebookInstanceLifecycleConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeNotebookInstanceLifecycleConfig) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeNotebookInstanceLifecycleConfig) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeNotebookInstanceLifecycleConfig) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeNotebookInstanceLifecycleConfig) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeNotebookInstanceLifecycleConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeNotebookInstanceLifecycleConfig) 