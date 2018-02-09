# StartNotebookInstance<a name="API_StartNotebookInstance"></a>

Launches an ML compute instance with the latest version of the libraries and attaches your ML storage volume\. After configuring the notebook instance, Amazon SageMaker sets the notebook instance status to `InService`\. A notebook instance's status must be `InService` before you can connect to your Jupyter notebook\. 

## Request Syntax<a name="API_StartNotebookInstance_RequestSyntax"></a>

```
{
   "NotebookInstanceName": "string"
}
```

## Request Parameters<a name="API_StartNotebookInstance_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** NotebookInstanceName **   
The name of the notebook instance to start\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Elements<a name="API_StartNotebookInstance_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_StartNotebookInstance_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_StartNotebookInstance_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/StartNotebookInstance) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/StartNotebookInstance) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/StartNotebookInstance) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/StartNotebookInstance) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/StartNotebookInstance) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/StartNotebookInstance) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/StartNotebookInstance) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/StartNotebookInstance) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/StartNotebookInstance) 