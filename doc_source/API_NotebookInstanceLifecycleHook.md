# NotebookInstanceLifecycleHook<a name="API_NotebookInstanceLifecycleHook"></a>

Contains the notebook instance lifecycle configuration script\.

Each lifecycle configuration script has a limit of 16384 characters\.

The value of the `$PATH` environment variable that is available to both scripts is `/sbin:bin:/usr/sbin:/usr/bin`\.

View CloudWatch Logs for notebook instance lifecycle configurations in log group `/aws/sagemaker/NotebookInstances` in log stream `[notebook-instance-name]/[LifecycleConfigHook]`\.

Lifecycle configuration scripts cannot run for longer than 5 minutes\. If a script runs for longer than 5 minutes, it fails and the notebook instance is not created or started\.

For information about notebook instance lifestyle configurations, see [Step 2\.1: \(Optional\) Customize a Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/notebook-lifecycle-config.html)\.

## Contents<a name="API_NotebookInstanceLifecycleHook_Contents"></a>

 **Content**   <a name="SageMaker-Type-NotebookInstanceLifecycleHook-Content"></a>
A base64\-encoded string that contains a shell script for a notebook instance lifecycle configuration\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 16384\.  
Required: No

## See Also<a name="API_NotebookInstanceLifecycleHook_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/NotebookInstanceLifecycleHook) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/NotebookInstanceLifecycleHook) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/NotebookInstanceLifecycleHook) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/NotebookInstanceLifecycleHook) 