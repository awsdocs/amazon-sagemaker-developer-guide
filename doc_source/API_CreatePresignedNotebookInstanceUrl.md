# CreatePresignedNotebookInstanceUrl<a name="API_CreatePresignedNotebookInstanceUrl"></a>

Returns a URL that you can use to connect to the Jupyter server from a notebook instance\. In the Amazon SageMaker console, when you choose `Open` next to a notebook instance, Amazon SageMaker opens a new tab showing the Jupyter server home page from the notebook instance\. The console uses this API to get the URL and show the page\.

You can restrict access to this API and to the URL that it returns to a list of IP addresses that you specify\. To restrict access, attach an IAM policy that denies access to this API unless the call comes from an IP address in the specified list to every AWS Identity and Access Management user, group, or role used to access the notebook instance\. Use the `NotIpAddress` condition operator and the `aws:SourceIP` condition context key to specify the list of IP addresses that you want to have access to the notebook instance\. For more information, see [Limit Access to a Notebook Instance by IP Address](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-ip-filter.html)\.

**Note**  
The URL that you get from a call to [CreatePresignedNotebookInstanceUrl](#API_CreatePresignedNotebookInstanceUrl) is valid only for 5 minutes\. If you try to use the URL after the 5\-minute limit expires, you are directed to the AWS console sign\-in page\.

## Request Syntax<a name="API_CreatePresignedNotebookInstanceUrl_RequestSyntax"></a>

```
{
   "[NotebookInstanceName](#SageMaker-CreatePresignedNotebookInstanceUrl-request-NotebookInstanceName)": "string",
   "[SessionExpirationDurationInSeconds](#SageMaker-CreatePresignedNotebookInstanceUrl-request-SessionExpirationDurationInSeconds)": number
}
```

## Request Parameters<a name="API_CreatePresignedNotebookInstanceUrl_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [NotebookInstanceName](#API_CreatePresignedNotebookInstanceUrl_RequestSyntax) **   <a name="SageMaker-CreatePresignedNotebookInstanceUrl-request-NotebookInstanceName"></a>
The name of the notebook instance\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [SessionExpirationDurationInSeconds](#API_CreatePresignedNotebookInstanceUrl_RequestSyntax) **   <a name="SageMaker-CreatePresignedNotebookInstanceUrl-request-SessionExpirationDurationInSeconds"></a>
The duration of the session, in seconds\. The default is 12 hours\.  
Type: Integer  
Valid Range: Minimum value of 1800\. Maximum value of 43200\.  
Required: No

## Response Syntax<a name="API_CreatePresignedNotebookInstanceUrl_ResponseSyntax"></a>

```
{
   "[AuthorizedUrl](#SageMaker-CreatePresignedNotebookInstanceUrl-response-AuthorizedUrl)": "string"
}
```

## Response Elements<a name="API_CreatePresignedNotebookInstanceUrl_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AuthorizedUrl](#API_CreatePresignedNotebookInstanceUrl_ResponseSyntax) **   <a name="SageMaker-CreatePresignedNotebookInstanceUrl-response-AuthorizedUrl"></a>
A JSON object that contains the URL string\.   
Type: String

## Errors<a name="API_CreatePresignedNotebookInstanceUrl_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_CreatePresignedNotebookInstanceUrl_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreatePresignedNotebookInstanceUrl) 