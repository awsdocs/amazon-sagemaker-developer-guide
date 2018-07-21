# ListHyperParameterTuningJobs<a name="API_ListHyperParameterTuningJobs"></a>

Gets a list of [HyperParameterTuningJobSummary](API_HyperParameterTuningJobSummary.md) objects that describe the hyperparameter tuning jobs launched in your account\.

## Request Syntax<a name="API_ListHyperParameterTuningJobs_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListHyperParameterTuningJobs-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListHyperParameterTuningJobs-request-CreationTimeBefore)": number,
   "[LastModifiedTimeAfter](#SageMaker-ListHyperParameterTuningJobs-request-LastModifiedTimeAfter)": number,
   "[LastModifiedTimeBefore](#SageMaker-ListHyperParameterTuningJobs-request-LastModifiedTimeBefore)": number,
   "[MaxResults](#SageMaker-ListHyperParameterTuningJobs-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListHyperParameterTuningJobs-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListHyperParameterTuningJobs-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListHyperParameterTuningJobs-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListHyperParameterTuningJobs-request-SortOrder)": "string",
   "[StatusEquals](#SageMaker-ListHyperParameterTuningJobs-request-StatusEquals)": "string"
}
```

## Request Parameters<a name="API_ListHyperParameterTuningJobs_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-CreationTimeAfter"></a>
A filter that returns only tuning jobs that were created after the specified time\.  
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-CreationTimeBefore"></a>
A filter that returns only tuning jobs that were created before the specified time\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeAfter](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-LastModifiedTimeAfter"></a>
A filter that returns only tuning jobs that were modified after the specified time\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeBefore](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-LastModifiedTimeBefore"></a>
A filter that returns only tuning jobs that were modified before the specified time\.  
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-MaxResults"></a>
The maximum number of tuning jobs to return\. The default value is 10\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-NameContains"></a>
A string in the tuning job name\. This filter returns only tuning jobs whose name contains the specified string\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9\-]+`   
Required: No

 ** [NextToken](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-NextToken"></a>
If the result of the previous `ListHyperParameterTuningJobs` request was truncated, the response includes a `NextToken`\. To retrieve the next set of tuning jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [SortBy](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-SortBy"></a>
The field to sort results by\. The default is `Name`\.  
Type: String  
Valid Values:` Name | Status | CreationTime`   
Required: No

 ** [SortOrder](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-SortOrder"></a>
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** [StatusEquals](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-StatusEquals"></a>
A filter that returns only tuning jobs with the specified status\.  
Type: String  
Valid Values:` Completed | InProgress | Failed | Stopped | Stopping`   
Required: No

## Response Syntax<a name="API_ListHyperParameterTuningJobs_ResponseSyntax"></a>

```
{
   "[HyperParameterTuningJobSummaries](#SageMaker-ListHyperParameterTuningJobs-response-HyperParameterTuningJobSummaries)": [ 
      { 
         "[CreationTime](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-CreationTime)": number,
         "[HyperParameterTuningEndTime](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningEndTime)": number,
         "[HyperParameterTuningJobArn](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningJobArn)": "string",
         "[HyperParameterTuningJobName](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningJobName)": "string",
         "[HyperParameterTuningJobStatus](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningJobStatus)": "string",
         "[LastModifiedTime](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-LastModifiedTime)": number,
         "[ObjectiveStatusCounters](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-ObjectiveStatusCounters)": { 
            "[Failed](API_ObjectiveStatusCounters.md#SageMaker-Type-ObjectiveStatusCounters-Failed)": number,
            "[Pending](API_ObjectiveStatusCounters.md#SageMaker-Type-ObjectiveStatusCounters-Pending)": number,
            "[Succeeded](API_ObjectiveStatusCounters.md#SageMaker-Type-ObjectiveStatusCounters-Succeeded)": number
         },
         "[ResourceLimits](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-ResourceLimits)": { 
            "[MaxNumberOfTrainingJobs](API_ResourceLimits.md#SageMaker-Type-ResourceLimits-MaxNumberOfTrainingJobs)": number,
            "[MaxParallelTrainingJobs](API_ResourceLimits.md#SageMaker-Type-ResourceLimits-MaxParallelTrainingJobs)": number
         },
         "[Strategy](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-Strategy)": "string",
         "[TrainingJobStatusCounters](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-TrainingJobStatusCounters)": { 
            "[Completed](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-Completed)": number,
            "[InProgress](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-InProgress)": number,
            "[NonRetryableError](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-NonRetryableError)": number,
            "[RetryableError](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-RetryableError)": number,
            "[Stopped](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-Stopped)": number
         }
      }
   ],
   "[NextToken](#SageMaker-ListHyperParameterTuningJobs-response-NextToken)": "string"
}
```

## Response Elements<a name="API_ListHyperParameterTuningJobs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [HyperParameterTuningJobSummaries](#API_ListHyperParameterTuningJobs_ResponseSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-response-HyperParameterTuningJobSummaries"></a>
A list of [HyperParameterTuningJobSummary](API_HyperParameterTuningJobSummary.md) objects that describe the tuning jobs that the `ListHyperParameterTuningJobs` request returned\.  
Type: Array of [HyperParameterTuningJobSummary](API_HyperParameterTuningJobSummary.md) objects

 ** [NextToken](#API_ListHyperParameterTuningJobs_ResponseSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-response-NextToken"></a>
If the result of this `ListHyperParameterTuningJobs` request was truncated, the response includes a `NextToken`\. To retrieve the next set of tuning jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.

## Errors<a name="API_ListHyperParameterTuningJobs_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListHyperParameterTuningJobs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 