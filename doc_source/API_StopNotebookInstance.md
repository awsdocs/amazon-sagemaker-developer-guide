# StopNotebookInstance<a name="API_StopNotebookInstance"></a>

Terminates the ML compute instance\. Before terminating the instance, Amazon SageMaker disconnects the ML storage volume from it\. Amazon SageMaker preserves the ML storage volume\. Amazon Amazon SageMaker stops charing you for the ML compute instance when you call `StopNotebookInstance`\.

To access data on the ML storage volume for a notebook instance that has been terminated, call the `StartNotebookInstance` API\. `StartNotebookInstance` launches another ML compute instance, configures it, and attaches the preserved ML storage volume so you can continue your work\. 

## Request Syntax<a name="API_StopNotebookInstance_RequestSyntax"></a>

```
{
   "[NotebookInstanceName](#SageMaker-StopNotebookInstance-request-NotebookInstanceName)": "string"
}
```

## Request Parameters<a name="API_StopNotebookInstance_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [NotebookInstanceName](#API_StopNotebookInstance_RequestSyntax) **   <a name="SageMaker-StopNotebookInstance-request-NotebookInstanceName"></a>
The name of the notebook instance to terminate\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Elements<a name="API_StopNotebookInstance_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_StopNotebookInstance_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_StopNotebookInstance_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/StopNotebookInstance) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/StopNotebookInstance) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/StopNotebookInstance) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/StopNotebookInstance) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/StopNotebookInstance) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/StopNotebookInstance) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/StopNotebookInstance) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/StopNotebookInstance) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/StopNotebookInstance) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/StopNotebookInstance) 