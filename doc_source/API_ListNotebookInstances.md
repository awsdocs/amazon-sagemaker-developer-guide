# ListNotebookInstances<a name="API_ListNotebookInstances"></a>

Returns a list of the Amazon SageMaker notebook instances in the requester's account in an AWS Region\. 

## Request Syntax<a name="API_ListNotebookInstances_RequestSyntax"></a>

```
{
   "[AdditionalCodeRepositoryEquals](#SageMaker-ListNotebookInstances-request-AdditionalCodeRepositoryEquals)": "string",
   "[CreationTimeAfter](#SageMaker-ListNotebookInstances-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListNotebookInstances-request-CreationTimeBefore)": number,
   "[DefaultCodeRepositoryContains](#SageMaker-ListNotebookInstances-request-DefaultCodeRepositoryContains)": "string",
   "[LastModifiedTimeAfter](#SageMaker-ListNotebookInstances-request-LastModifiedTimeAfter)": number,
   "[LastModifiedTimeBefore](#SageMaker-ListNotebookInstances-request-LastModifiedTimeBefore)": number,
   "[MaxResults](#SageMaker-ListNotebookInstances-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListNotebookInstances-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListNotebookInstances-request-NextToken)": "string",
   "[NotebookInstanceLifecycleConfigNameContains](#SageMaker-ListNotebookInstances-request-NotebookInstanceLifecycleConfigNameContains)": "string",
   "[SortBy](#SageMaker-ListNotebookInstances-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListNotebookInstances-request-SortOrder)": "string",
   "[StatusEquals](#SageMaker-ListNotebookInstances-request-StatusEquals)": "string"
}
```

## Request Parameters<a name="API_ListNotebookInstances_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [AdditionalCodeRepositoryEquals](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-AdditionalCodeRepositoryEquals"></a>
A filter that returns only notebook instances with associated with the specified git repository\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `^https://([^/]+)/?(.*)$|^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [CreationTimeAfter](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-CreationTimeAfter"></a>
A filter that returns only notebook instances that were created after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-CreationTimeBefore"></a>
A filter that returns only notebook instances that were created before the specified time \(timestamp\)\.   
Type: Timestamp  
Required: No

 ** [DefaultCodeRepositoryContains](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-DefaultCodeRepositoryContains"></a>
A string in the name or URL of a Git repository associated with this notebook instance\. This filter returns only notebook instances associated with a git repository with a name that contains the specified string\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `[a-zA-Z0-9-]+`   
Required: No

 ** [LastModifiedTimeAfter](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-LastModifiedTimeAfter"></a>
A filter that returns only notebook instances that were modified after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeBefore](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-LastModifiedTimeBefore"></a>
A filter that returns only notebook instances that were modified before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-MaxResults"></a>
The maximum number of notebook instances to return\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-NameContains"></a>
A string in the notebook instances' name\. This filter returns only notebook instances whose name contains the specified string\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9-]+`   
Required: No

 ** [NextToken](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-NextToken"></a>
 If the previous call to the `ListNotebookInstances` is truncated, the response includes a `NextToken`\. You can use this token in your subsequent `ListNotebookInstances` request to fetch the next set of notebook instances\.   
You might specify a filter or a sort order in your request\. When response is truncated, you must use the same values for the filer and sort order in the next request\. 
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [NotebookInstanceLifecycleConfigNameContains](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-NotebookInstanceLifecycleConfigNameContains"></a>
A string in the name of a notebook instances lifecycle configuration associated with this notebook instance\. This filter returns only notebook instances associated with a lifecycle configuration with a name that contains the specified string\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [SortBy](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-SortBy"></a>
The field to sort results by\. The default is `Name`\.  
Type: String  
Valid Values:` Name | CreationTime | Status`   
Required: No

 ** [SortOrder](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-SortOrder"></a>
The sort order for results\.   
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** [StatusEquals](#API_ListNotebookInstances_RequestSyntax) **   <a name="SageMaker-ListNotebookInstances-request-StatusEquals"></a>
A filter that returns only notebook instances with the specified status\.  
Type: String  
Valid Values:` Pending | InService | Stopping | Stopped | Failed | Deleting | Updating`   
Required: No

## Response Syntax<a name="API_ListNotebookInstances_ResponseSyntax"></a>

```
{
   "[NextToken](#SageMaker-ListNotebookInstances-response-NextToken)": "string",
   "[NotebookInstances](#SageMaker-ListNotebookInstances-response-NotebookInstances)": [ 
      { 
         "[AdditionalCodeRepositories](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-AdditionalCodeRepositories)": [ "string" ],
         "[CreationTime](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-CreationTime)": number,
         "[DefaultCodeRepository](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-DefaultCodeRepository)": "string",
         "[InstanceType](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-InstanceType)": "string",
         "[LastModifiedTime](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-LastModifiedTime)": number,
         "[NotebookInstanceArn](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-NotebookInstanceArn)": "string",
         "[NotebookInstanceLifecycleConfigName](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-NotebookInstanceLifecycleConfigName)": "string",
         "[NotebookInstanceName](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-NotebookInstanceName)": "string",
         "[NotebookInstanceStatus](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-NotebookInstanceStatus)": "string",
         "[Url](API_NotebookInstanceSummary.md#SageMaker-Type-NotebookInstanceSummary-Url)": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListNotebookInstances_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NextToken](#API_ListNotebookInstances_ResponseSyntax) **   <a name="SageMaker-ListNotebookInstances-response-NextToken"></a>
If the response to the previous `ListNotebookInstances` request was truncated, Amazon SageMaker returns this token\. To retrieve the next set of notebook instances, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.

 ** [NotebookInstances](#API_ListNotebookInstances_ResponseSyntax) **   <a name="SageMaker-ListNotebookInstances-response-NotebookInstances"></a>
An array of `NotebookInstanceSummary` objects, one for each notebook instance\.  
Type: Array of [NotebookInstanceSummary](API_NotebookInstanceSummary.md) objects

## Errors<a name="API_ListNotebookInstances_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListNotebookInstances_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListNotebookInstances) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListNotebookInstances) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListNotebookInstances) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListNotebookInstances) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListNotebookInstances) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListNotebookInstances) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListNotebookInstances) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListNotebookInstances) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListNotebookInstances) 