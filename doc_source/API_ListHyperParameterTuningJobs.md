# ListHyperParameterTuningJobs<a name="API_ListHyperParameterTuningJobs"></a>

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
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-CreationTimeBefore"></a>
Type: Timestamp  
Required: No

 ** [LastModifiedTimeAfter](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-LastModifiedTimeAfter"></a>
Type: Timestamp  
Required: No

 ** [LastModifiedTimeBefore](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-LastModifiedTimeBefore"></a>
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-MaxResults"></a>
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-NameContains"></a>
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9\-]+`   
Required: No

 ** [NextToken](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-NextToken"></a>
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [SortBy](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-SortBy"></a>
Type: String  
Valid Values:` Name | Status | CreationTime`   
Required: No

 ** [SortOrder](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-SortOrder"></a>
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** [StatusEquals](#API_ListHyperParameterTuningJobs_RequestSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-request-StatusEquals"></a>
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
         "[ResourceLimits](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-ResourceLimits)": { 
            "[MaxNumberOfTrainingJobs](API_ResourceLimits.md#SageMaker-Type-ResourceLimits-MaxNumberOfTrainingJobs)": number,
            "[MaxParallelTrainingJobs](API_ResourceLimits.md#SageMaker-Type-ResourceLimits-MaxParallelTrainingJobs)": number
         },
         "[Strategy](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-Strategy)": "string",
         "[TrainingJobCounters](API_HyperParameterTuningJobSummary.md#SageMaker-Type-HyperParameterTuningJobSummary-TrainingJobCounters)": { 
            "[ClientError](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-ClientError)": number,
            "[Completed](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-Completed)": number,
            "[Fault](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-Fault)": number,
            "[InProgress](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-InProgress)": number,
            "[Stopped](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-Stopped)": number
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
Type: Array of [HyperParameterTuningJobSummary](API_HyperParameterTuningJobSummary.md) objects

 ** [NextToken](#API_ListHyperParameterTuningJobs_ResponseSyntax) **   <a name="SageMaker-ListHyperParameterTuningJobs-response-NextToken"></a>
Type: String  
Length Constraints: Maximum length of 8192\.

## Errors<a name="API_ListHyperParameterTuningJobs_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListHyperParameterTuningJobs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListHyperParameterTuningJobs) 