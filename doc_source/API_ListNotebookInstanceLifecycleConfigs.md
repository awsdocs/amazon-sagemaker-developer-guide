# ListNotebookInstanceLifecycleConfigs<a name="API_ListNotebookInstanceLifecycleConfigs"></a>

Lists notebook instance lifestyle configurations created with the [CreateNotebookInstanceLifecycleConfig](API_CreateNotebookInstanceLifecycleConfig.md) API\.

## Request Syntax<a name="API_ListNotebookInstanceLifecycleConfigs_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListNotebookInstanceLifecycleConfigs-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListNotebookInstanceLifecycleConfigs-request-CreationTimeBefore)": number,
   "[LastModifiedTimeAfter](#SageMaker-ListNotebookInstanceLifecycleConfigs-request-LastModifiedTimeAfter)": number,
   "[LastModifiedTimeBefore](#SageMaker-ListNotebookInstanceLifecycleConfigs-request-LastModifiedTimeBefore)": number,
   "[MaxResults](#SageMaker-ListNotebookInstanceLifecycleConfigs-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListNotebookInstanceLifecycleConfigs-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListNotebookInstanceLifecycleConfigs-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListNotebookInstanceLifecycleConfigs-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListNotebookInstanceLifecycleConfigs-request-SortOrder)": "string"
}
```

## Request Parameters<a name="API_ListNotebookInstanceLifecycleConfigs_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListNotebookInstanceLifecycleConfigs_RequestSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-request-CreationTimeAfter"></a>
A filter that returns only lifecycle configurations that were created after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListNotebookInstanceLifecycleConfigs_RequestSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-request-CreationTimeBefore"></a>
A filter that returns only lifecycle configurations that were created before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeAfter](#API_ListNotebookInstanceLifecycleConfigs_RequestSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-request-LastModifiedTimeAfter"></a>
A filter that returns only lifecycle configurations that were modified after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeBefore](#API_ListNotebookInstanceLifecycleConfigs_RequestSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-request-LastModifiedTimeBefore"></a>
A filter that returns only lifecycle configurations that were modified before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListNotebookInstanceLifecycleConfigs_RequestSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-request-MaxResults"></a>
The maximum number of lifecycle configurations to return in the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListNotebookInstanceLifecycleConfigs_RequestSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-request-NameContains"></a>
A string in the lifecycle configuration name\. This filter returns only lifecycle configurations whose name contains the specified string\.  
Type: String  
Pattern: `[a-zA-Z0-9-]+`   
Required: No

 ** [NextToken](#API_ListNotebookInstanceLifecycleConfigs_RequestSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-request-NextToken"></a>
If the result of a `ListNotebookInstanceLifecycleConfigs` request was truncated, the response includes a `NextToken`\. To get the next set of lifecycle configurations, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [SortBy](#API_ListNotebookInstanceLifecycleConfigs_RequestSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-request-SortBy"></a>
Sorts the list of results\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreationTime | LastModifiedTime`   
Required: No

 ** [SortOrder](#API_ListNotebookInstanceLifecycleConfigs_RequestSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-request-SortOrder"></a>
The sort order for results\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

## Response Syntax<a name="API_ListNotebookInstanceLifecycleConfigs_ResponseSyntax"></a>

```
{
   "[NextToken](#SageMaker-ListNotebookInstanceLifecycleConfigs-response-NextToken)": "string",
   "[NotebookInstanceLifecycleConfigs](#SageMaker-ListNotebookInstanceLifecycleConfigs-response-NotebookInstanceLifecycleConfigs)": [ 
      { 
         "[CreationTime](API_NotebookInstanceLifecycleConfigSummary.md#SageMaker-Type-NotebookInstanceLifecycleConfigSummary-CreationTime)": number,
         "[LastModifiedTime](API_NotebookInstanceLifecycleConfigSummary.md#SageMaker-Type-NotebookInstanceLifecycleConfigSummary-LastModifiedTime)": number,
         "[NotebookInstanceLifecycleConfigArn](API_NotebookInstanceLifecycleConfigSummary.md#SageMaker-Type-NotebookInstanceLifecycleConfigSummary-NotebookInstanceLifecycleConfigArn)": "string",
         "[NotebookInstanceLifecycleConfigName](API_NotebookInstanceLifecycleConfigSummary.md#SageMaker-Type-NotebookInstanceLifecycleConfigSummary-NotebookInstanceLifecycleConfigName)": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListNotebookInstanceLifecycleConfigs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NextToken](#API_ListNotebookInstanceLifecycleConfigs_ResponseSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this token\. To get the next set of lifecycle configurations, use it in the next request\.   
Type: String  
Length Constraints: Maximum length of 8192\.

 ** [NotebookInstanceLifecycleConfigs](#API_ListNotebookInstanceLifecycleConfigs_ResponseSyntax) **   <a name="SageMaker-ListNotebookInstanceLifecycleConfigs-response-NotebookInstanceLifecycleConfigs"></a>
An array of `NotebookInstanceLifecycleConfiguration` objects, each listing a lifecycle configuration\.  
Type: Array of [NotebookInstanceLifecycleConfigSummary](API_NotebookInstanceLifecycleConfigSummary.md) objects

## Errors<a name="API_ListNotebookInstanceLifecycleConfigs_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListNotebookInstanceLifecycleConfigs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListNotebookInstanceLifecycleConfigs) 
+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListNotebookInstanceLifecycleConfigs) 
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListNotebookInstanceLifecycleConfigs) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListNotebookInstanceLifecycleConfigs) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListNotebookInstanceLifecycleConfigs) 
+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListNotebookInstanceLifecycleConfigs) 
+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListNotebookInstanceLifecycleConfigs) 
+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListNotebookInstanceLifecycleConfigs) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListNotebookInstanceLifecycleConfigs) 