# ListLabelingJobs<a name="API_ListLabelingJobs"></a>

Gets a list of labeling jobs\.

## Request Syntax<a name="API_ListLabelingJobs_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListLabelingJobs-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListLabelingJobs-request-CreationTimeBefore)": number,
   "[LastModifiedTimeAfter](#SageMaker-ListLabelingJobs-request-LastModifiedTimeAfter)": number,
   "[LastModifiedTimeBefore](#SageMaker-ListLabelingJobs-request-LastModifiedTimeBefore)": number,
   "[MaxResults](#SageMaker-ListLabelingJobs-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListLabelingJobs-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListLabelingJobs-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListLabelingJobs-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListLabelingJobs-request-SortOrder)": "string",
   "[StatusEquals](#SageMaker-ListLabelingJobs-request-StatusEquals)": "string"
}
```

## Request Parameters<a name="API_ListLabelingJobs_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-CreationTimeAfter"></a>
A filter that returns only labeling jobs created after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-CreationTimeBefore"></a>
A filter that returns only labeling jobs created before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeAfter](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-LastModifiedTimeAfter"></a>
A filter that returns only labeling jobs modified after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [LastModifiedTimeBefore](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-LastModifiedTimeBefore"></a>
A filter that returns only labeling jobs modified before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-MaxResults"></a>
The maximum number of labeling jobs to return in each page of the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-NameContains"></a>
A string in the labeling job name\. This filter returns only labeling jobs whose name contains the specified string\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9\-]+`   
Required: No

 ** [NextToken](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-NextToken"></a>
If the result of the previous `ListLabelingJobs` request was truncated, the response includes a `NextToken`\. To retrieve the next set of labeling jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*`   
Required: No

 ** [SortBy](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-SortBy"></a>
The field to sort results by\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreationTime | Status`   
Required: No

 ** [SortOrder](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-SortOrder"></a>
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** [StatusEquals](#API_ListLabelingJobs_RequestSyntax) **   <a name="SageMaker-ListLabelingJobs-request-StatusEquals"></a>
A filter that retrieves only labeling jobs with a specific status\.  
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: No

## Response Syntax<a name="API_ListLabelingJobs_ResponseSyntax"></a>

```
{
   "[LabelingJobSummaryList](#SageMaker-ListLabelingJobs-response-LabelingJobSummaryList)": [ 
      { 
         "[AnnotationConsolidationLambdaArn](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-AnnotationConsolidationLambdaArn)": "string",
         "[CreationTime](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-CreationTime)": number,
         "[FailureReason](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-FailureReason)": "string",
         "[InputConfig](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-InputConfig)": { 
            "[DataAttributes](API_LabelingJobInputConfig.md#SageMaker-Type-LabelingJobInputConfig-DataAttributes)": { 
               "[ContentClassifiers](API_LabelingJobDataAttributes.md#SageMaker-Type-LabelingJobDataAttributes-ContentClassifiers)": [ "string" ]
            },
            "[DataSource](API_LabelingJobInputConfig.md#SageMaker-Type-LabelingJobInputConfig-DataSource)": { 
               "[S3DataSource](API_LabelingJobDataSource.md#SageMaker-Type-LabelingJobDataSource-S3DataSource)": { 
                  "[ManifestS3Uri](API_LabelingJobS3DataSource.md#SageMaker-Type-LabelingJobS3DataSource-ManifestS3Uri)": "string"
               }
            }
         },
         "[LabelCounters](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-LabelCounters)": { 
            "[FailedNonRetryableError](API_LabelCounters.md#SageMaker-Type-LabelCounters-FailedNonRetryableError)": number,
            "[HumanLabeled](API_LabelCounters.md#SageMaker-Type-LabelCounters-HumanLabeled)": number,
            "[MachineLabeled](API_LabelCounters.md#SageMaker-Type-LabelCounters-MachineLabeled)": number,
            "[TotalLabeled](API_LabelCounters.md#SageMaker-Type-LabelCounters-TotalLabeled)": number,
            "[Unlabeled](API_LabelCounters.md#SageMaker-Type-LabelCounters-Unlabeled)": number
         },
         "[LabelingJobArn](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-LabelingJobArn)": "string",
         "[LabelingJobName](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-LabelingJobName)": "string",
         "[LabelingJobOutput](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-LabelingJobOutput)": { 
            "[FinalActiveLearningModelArn](API_LabelingJobOutput.md#SageMaker-Type-LabelingJobOutput-FinalActiveLearningModelArn)": "string",
            "[OutputDatasetS3Uri](API_LabelingJobOutput.md#SageMaker-Type-LabelingJobOutput-OutputDatasetS3Uri)": "string"
         },
         "[LabelingJobStatus](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-LabelingJobStatus)": "string",
         "[LastModifiedTime](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-LastModifiedTime)": number,
         "[PreHumanTaskLambdaArn](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-PreHumanTaskLambdaArn)": "string",
         "[WorkteamArn](API_LabelingJobSummary.md#SageMaker-Type-LabelingJobSummary-WorkteamArn)": "string"
      }
   ],
   "[NextToken](#SageMaker-ListLabelingJobs-response-NextToken)": "string"
}
```

## Response Elements<a name="API_ListLabelingJobs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [LabelingJobSummaryList](#API_ListLabelingJobs_ResponseSyntax) **   <a name="SageMaker-ListLabelingJobs-response-LabelingJobSummaryList"></a>
An array of `LabelingJobSummary` objects, each describing a labeling job\.  
Type: Array of [LabelingJobSummary](API_LabelingJobSummary.md) objects

 ** [NextToken](#API_ListLabelingJobs_ResponseSyntax) **   <a name="SageMaker-ListLabelingJobs-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of labeling jobs, use it in the subsequent request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*` 

## Errors<a name="API_ListLabelingJobs_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListLabelingJobs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListLabelingJobs) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListLabelingJobs) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListLabelingJobs) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListLabelingJobs) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ListLabelingJobs) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListLabelingJobs) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListLabelingJobs) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListLabelingJobs) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListLabelingJobs) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListLabelingJobs) 