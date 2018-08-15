# ListTrainingJobs<a name="API_ListTrainingJobs"></a>

Lists training jobs\.

## Request Syntax<a name="API_ListTrainingJobs_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListTrainingJobs-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListTrainingJobs-request-CreationTimeBefore)": number,
   "[LastModifiedTimeAfter](#SageMaker-ListTrainingJobs-request-LastModifiedTimeAfter)": number,
   "[LastModifiedTimeBefore](#SageMaker-ListTrainingJobs-request-LastModifiedTimeBefore)": number,
   "[MaxResults](#SageMaker-ListTrainingJobs-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListTrainingJobs-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListTrainingJobs-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListTrainingJobs-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListTrainingJobs-request-SortOrder)": "string",
   "[StatusEquals](#SageMaker-ListTrainingJobs-request-StatusEquals)": "string"
}
```

## Request Parameters<a name="API_ListTrainingJobs_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-CreationTimeAfter"></a>
A filter that returns only training jobs created after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-CreationTimeBefore"></a>
A filter that returns only training jobs created before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeAfter](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-LastModifiedTimeAfter"></a>
A filter that returns only training jobs modified after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeBefore](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-LastModifiedTimeBefore"></a>
A filter that returns only training jobs modified before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-MaxResults"></a>
The maximum number of training jobs to return in the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-NameContains"></a>
A string in the training job name\. This filter returns only training jobs whose name contains the specified string\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9\-]+`   
Required: No

 ** [NextToken](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-NextToken"></a>
If the result of the previous `ListTrainingJobs` request was truncated, the response includes a `NextToken`\. To retrieve the next set of training jobs, use the token in the next request\.   
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [SortBy](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-SortBy"></a>
The field to sort results by\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreationTime | Status`   
Required: No

 ** [SortOrder](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-SortOrder"></a>
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** [StatusEquals](#API_ListTrainingJobs_RequestSyntax) **   <a name="SageMaker-ListTrainingJobs-request-StatusEquals"></a>
A filter that retrieves only training jobs with a specific status\.  
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: No

## Response Syntax<a name="API_ListTrainingJobs_ResponseSyntax"></a>

```
{
   "[NextToken](#SageMaker-ListTrainingJobs-response-NextToken)": "string",
   "[TrainingJobSummaries](#SageMaker-ListTrainingJobs-response-TrainingJobSummaries)": [ 
      { 
         "[CreationTime](API_TrainingJobSummary.md#SageMaker-Type-TrainingJobSummary-CreationTime)": number,
         "[LastModifiedTime](API_TrainingJobSummary.md#SageMaker-Type-TrainingJobSummary-LastModifiedTime)": number,
         "[TrainingEndTime](API_TrainingJobSummary.md#SageMaker-Type-TrainingJobSummary-TrainingEndTime)": number,
         "[TrainingJobArn](API_TrainingJobSummary.md#SageMaker-Type-TrainingJobSummary-TrainingJobArn)": "string",
         "[TrainingJobName](API_TrainingJobSummary.md#SageMaker-Type-TrainingJobSummary-TrainingJobName)": "string",
         "[TrainingJobStatus](API_TrainingJobSummary.md#SageMaker-Type-TrainingJobSummary-TrainingJobStatus)": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListTrainingJobs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NextToken](#API_ListTrainingJobs_ResponseSyntax) **   <a name="SageMaker-ListTrainingJobs-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of training jobs, use it in the subsequent request\.  
Type: String  
Length Constraints: Maximum length of 8192\.

 ** [TrainingJobSummaries](#API_ListTrainingJobs_ResponseSyntax) **   <a name="SageMaker-ListTrainingJobs-response-TrainingJobSummaries"></a>
An array of `TrainingJobSummary` objects, each listing a training job\.  
Type: Array of [TrainingJobSummary](API_TrainingJobSummary.md) objects

## Errors<a name="API_ListTrainingJobs_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListTrainingJobs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListTrainingJobs) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListTrainingJobs) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListTrainingJobs) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListTrainingJobs) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListTrainingJobs) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListTrainingJobs) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListTrainingJobs) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListTrainingJobs) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListTrainingJobs) 