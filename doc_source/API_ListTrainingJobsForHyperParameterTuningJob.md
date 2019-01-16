# ListTrainingJobsForHyperParameterTuningJob<a name="API_ListTrainingJobsForHyperParameterTuningJob"></a>

Gets a list of [TrainingJobSummary](API_TrainingJobSummary.md) objects that describe the training jobs that a hyperparameter tuning job launched\.

## Request Syntax<a name="API_ListTrainingJobsForHyperParameterTuningJob_RequestSyntax"></a>

```
{
   "[HyperParameterTuningJobName](#SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-HyperParameterTuningJobName)": "string",
   "[MaxResults](#SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-MaxResults)": number,
   "[NextToken](#SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-SortOrder)": "string",
   "[StatusEquals](#SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-StatusEquals)": "string"
}
```

## Request Parameters<a name="API_ListTrainingJobsForHyperParameterTuningJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [HyperParameterTuningJobName](#API_ListTrainingJobsForHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-HyperParameterTuningJobName"></a>
The name of the tuning job whose training jobs you want to list\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [MaxResults](#API_ListTrainingJobsForHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-MaxResults"></a>
The maximum number of training jobs to return\. The default value is 10\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NextToken](#API_ListTrainingJobsForHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-NextToken"></a>
If the result of the previous `ListTrainingJobsForHyperParameterTuningJob` request was truncated, the response includes a `NextToken`\. To retrieve the next set of training jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [SortBy](#API_ListTrainingJobsForHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-SortBy"></a>
The field to sort results by\. The default is `Name`\.  
If the value of this field is `FinalObjectiveMetricValue`, any training jobs that did not return an objective metric are not listed\.  
Type: String  
Valid Values:` Name | CreationTime | Status | FinalObjectiveMetricValue`   
Required: No

 ** [SortOrder](#API_ListTrainingJobsForHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-SortOrder"></a>
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** [StatusEquals](#API_ListTrainingJobsForHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-ListTrainingJobsForHyperParameterTuningJob-request-StatusEquals"></a>
A filter that returns only training jobs with the specified status\.  
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: No

## Response Syntax<a name="API_ListTrainingJobsForHyperParameterTuningJob_ResponseSyntax"></a>

```
{
   "[NextToken](#SageMaker-ListTrainingJobsForHyperParameterTuningJob-response-NextToken)": "string",
   "[TrainingJobSummaries](#SageMaker-ListTrainingJobsForHyperParameterTuningJob-response-TrainingJobSummaries)": [ 
      { 
         "[CreationTime](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-CreationTime)": number,
         "[FailureReason](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-FailureReason)": "string",
         "[FinalHyperParameterTuningJobObjectiveMetric](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-FinalHyperParameterTuningJobObjectiveMetric)": { 
            "[MetricName](API_FinalHyperParameterTuningJobObjectiveMetric.md#SageMaker-Type-FinalHyperParameterTuningJobObjectiveMetric-MetricName)": "string",
            "[Type](API_FinalHyperParameterTuningJobObjectiveMetric.md#SageMaker-Type-FinalHyperParameterTuningJobObjectiveMetric-Type)": "string",
            "[Value](API_FinalHyperParameterTuningJobObjectiveMetric.md#SageMaker-Type-FinalHyperParameterTuningJobObjectiveMetric-Value)": number
         },
         "[ObjectiveStatus](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-ObjectiveStatus)": "string",
         "[TrainingEndTime](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TrainingEndTime)": number,
         "[TrainingJobArn](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobArn)": "string",
         "[TrainingJobName](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobName)": "string",
         "[TrainingJobStatus](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobStatus)": "string",
         "[TrainingStartTime](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TrainingStartTime)": number,
         "[TunedHyperParameters](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TunedHyperParameters)": { 
            "string" : "string" 
         },
         "[TuningJobName](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TuningJobName)": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListTrainingJobsForHyperParameterTuningJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NextToken](#API_ListTrainingJobsForHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-ListTrainingJobsForHyperParameterTuningJob-response-NextToken"></a>
If the result of this `ListTrainingJobsForHyperParameterTuningJob` request was truncated, the response includes a `NextToken`\. To retrieve the next set of training jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.

 ** [TrainingJobSummaries](#API_ListTrainingJobsForHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-ListTrainingJobsForHyperParameterTuningJob-response-TrainingJobSummaries"></a>
A list of [TrainingJobSummary](API_TrainingJobSummary.md) objects that describe the training jobs that the `ListTrainingJobsForHyperParameterTuningJob` request returned\.  
Type: Array of [HyperParameterTrainingJobSummary](API_HyperParameterTrainingJobSummary.md) objects

## Errors<a name="API_ListTrainingJobsForHyperParameterTuningJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_ListTrainingJobsForHyperParameterTuningJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListTrainingJobsForHyperParameterTuningJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListTrainingJobsForHyperParameterTuningJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListTrainingJobsForHyperParameterTuningJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListTrainingJobsForHyperParameterTuningJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListTrainingJobsForHyperParameterTuningJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListTrainingJobsForHyperParameterTuningJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListTrainingJobsForHyperParameterTuningJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListTrainingJobsForHyperParameterTuningJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListTrainingJobsForHyperParameterTuningJob) 