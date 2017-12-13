# CreatePresignedNotebookInstanceUrl<a name="API_CreatePresignedNotebookInstanceUrl"></a>

Returns a URL that you can use to connect to the Juypter server from a notebook instance\. In the Amazon SageMaker console, when you choose `Open` next to a notebook instance, Amazon SageMaker opens a new tab showing the Jupyter server home page from the notebook instance\. The console uses this API to get the URL and show the page\. 

## Request Syntax<a name="API_CreatePresignedNotebookInstanceUrl_RequestSyntax"></a>

```
{
   "NotebookInstanceName": "string",
   "SessionExpirationDurationInSeconds": number
}
```

## Request Parameters<a name="API_CreatePresignedNotebookInstanceUrl_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** NotebookInstanceName **   
The name of the notebook instance\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** SessionExpirationDurationInSeconds **   
The duration of the session, in seconds\. The default is 12 hours\.  
Type: Integer  
Valid Range: Minimum value of 1800\. Maximum value of 43200\.  
Required: No

## Response Syntax<a name="API_CreatePresignedNotebookInstanceUrl_ResponseSyntax"></a>

```
{
   "AuthorizedUrl": "string"
}
```

## Response Elements<a name="API_CreatePresignedNotebookInstanceUrl_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** AuthorizedUrl **   
A JSON object that contains the URL string\.   
Type: String

## Errors<a name="API_CreatePresignedNotebookInstanceUrl_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_CreatePresignedNotebookInstanceUrl_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 