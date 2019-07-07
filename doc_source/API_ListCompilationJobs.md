# ListCompilationJobs<a name="API_ListCompilationJobs"></a>

Lists model compilation jobs that satisfy various filters\.

To create a model compilation job, use [CreateCompilationJob](API_CreateCompilationJob.md)\. To get information about a particular model compilation job you have created, use [DescribeCompilationJob](API_DescribeCompilationJob.md)\.

## Request Syntax<a name="API_ListCompilationJobs_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListCompilationJobs-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListCompilationJobs-request-CreationTimeBefore)": number,
   "[LastModifiedTimeAfter](#SageMaker-ListCompilationJobs-request-LastModifiedTimeAfter)": number,
   "[LastModifiedTimeBefore](#SageMaker-ListCompilationJobs-request-LastModifiedTimeBefore)": number,
   "[MaxResults](#SageMaker-ListCompilationJobs-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListCompilationJobs-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListCompilationJobs-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListCompilationJobs-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListCompilationJobs-request-SortOrder)": "string",
   "[StatusEquals](#SageMaker-ListCompilationJobs-request-StatusEquals)": "string"
}
```

## Request Parameters<a name="API_ListCompilationJobs_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-CreationTimeAfter"></a>
A filter that returns the model compilation jobs that were created after a specified time\.   
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-CreationTimeBefore"></a>
A filter that returns the model compilation jobs that were created before a specified time\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeAfter](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-LastModifiedTimeAfter"></a>
A filter that returns the model compilation jobs that were modified after a specified time\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeBefore](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-LastModifiedTimeBefore"></a>
A filter that returns the model compilation jobs that were modified before a specified time\.  
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-MaxResults"></a>
The maximum number of model compilation jobs to return in the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-NameContains"></a>
A filter that returns the model compilation jobs whose name contains a specified string\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9\-]+`   
Required: No

 ** [NextToken](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-NextToken"></a>
If the result of the previous `ListCompilationJobs` request was truncated, the response includes a `NextToken`\. To retrieve the next set of model compilation jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*`   
Required: No

 ** [SortBy](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-SortBy"></a>
The field by which to sort results\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreationTime | Status`   
Required: No

 ** [SortOrder](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-SortOrder"></a>
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** [StatusEquals](#API_ListCompilationJobs_RequestSyntax) **   <a name="SageMaker-ListCompilationJobs-request-StatusEquals"></a>
A filter that retrieves model compilation jobs with a specific [DescribeCompilationJob:CompilationJobStatus](API_DescribeCompilationJob.md#SageMaker-DescribeCompilationJob-response-CompilationJobStatus) status\.  
Type: String  
Valid Values:` INPROGRESS | COMPLETED | FAILED | STARTING | STOPPING | STOPPED`   
Required: No

## Response Syntax<a name="API_ListCompilationJobs_ResponseSyntax"></a>

```
{
   "[CompilationJobSummaries](#SageMaker-ListCompilationJobs-response-CompilationJobSummaries)": [ 
      { 
         "[CompilationEndTime](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-CompilationEndTime)": number,
         "[CompilationJobArn](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-CompilationJobArn)": "string",
         "[CompilationJobName](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-CompilationJobName)": "string",
         "[CompilationJobStatus](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-CompilationJobStatus)": "string",
         "[CompilationStartTime](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-CompilationStartTime)": number,
         "[CompilationTargetDevice](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-CompilationTargetDevice)": "string",
         "[CreationTime](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-CreationTime)": number,
         "[LastModifiedTime](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-LastModifiedTime)": number
      }
   ],
   "[NextToken](#SageMaker-ListCompilationJobs-response-NextToken)": "string"
}
```

## Response Elements<a name="API_ListCompilationJobs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CompilationJobSummaries](#API_ListCompilationJobs_ResponseSyntax) **   <a name="SageMaker-ListCompilationJobs-response-CompilationJobSummaries"></a>
An array of [CompilationJobSummary](API_CompilationJobSummary.md) objects, each describing a model compilation job\.   
Type: Array of [CompilationJobSummary](API_CompilationJobSummary.md) objects

 ** [NextToken](#API_ListCompilationJobs_ResponseSyntax) **   <a name="SageMaker-ListCompilationJobs-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this `NextToken`\. To retrieve the next set of model compilation jobs, use this token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*` 

## Errors<a name="API_ListCompilationJobs_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListCompilationJobs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListCompilationJobs) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListCompilationJobs) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListCompilationJobs) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListCompilationJobs) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ListCompilationJobs) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListCompilationJobs) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListCompilationJobs) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListCompilationJobs) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListCompilationJobs) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListCompilationJobs) 