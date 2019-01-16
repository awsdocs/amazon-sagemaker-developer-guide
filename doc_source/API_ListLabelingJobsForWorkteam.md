# ListLabelingJobsForWorkteam<a name="API_ListLabelingJobsForWorkteam"></a>

Gets a list of labeling jobs assigned to a specified work team\.

## Request Syntax<a name="API_ListLabelingJobsForWorkteam_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListLabelingJobsForWorkteam-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListLabelingJobsForWorkteam-request-CreationTimeBefore)": number,
   "[JobReferenceCodeContains](#SageMaker-ListLabelingJobsForWorkteam-request-JobReferenceCodeContains)": "string",
   "[MaxResults](#SageMaker-ListLabelingJobsForWorkteam-request-MaxResults)": number,
   "[NextToken](#SageMaker-ListLabelingJobsForWorkteam-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListLabelingJobsForWorkteam-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListLabelingJobsForWorkteam-request-SortOrder)": "string",
   "[WorkteamArn](#SageMaker-ListLabelingJobsForWorkteam-request-WorkteamArn)": "string"
}
```

## Request Parameters<a name="API_ListLabelingJobsForWorkteam_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListLabelingJobsForWorkteam_RequestSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-request-CreationTimeAfter"></a>
A filter that returns only labeling jobs created after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListLabelingJobsForWorkteam_RequestSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-request-CreationTimeBefore"></a>
A filter that returns only labeling jobs created before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [JobReferenceCodeContains](#API_ListLabelingJobsForWorkteam_RequestSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-request-JobReferenceCodeContains"></a>
A filter the limits jobs to only the ones whose job reference code contains the specified string\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Required: No

 ** [MaxResults](#API_ListLabelingJobsForWorkteam_RequestSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-request-MaxResults"></a>
The maximum number of labeling jobs to return in each page of the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NextToken](#API_ListLabelingJobsForWorkteam_RequestSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-request-NextToken"></a>
If the result of the previous `ListLabelingJobsForWorkteam` request was truncated, the response includes a `NextToken`\. To retrieve the next set of labeling jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [SortBy](#API_ListLabelingJobsForWorkteam_RequestSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-request-SortBy"></a>
The field to sort results by\. The default is `CreationTime`\.  
Type: String  
Valid Values:` CreationTime`   
Required: No

 ** [SortOrder](#API_ListLabelingJobsForWorkteam_RequestSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-request-SortOrder"></a>
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** [WorkteamArn](#API_ListLabelingJobsForWorkteam_RequestSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-request-WorkteamArn"></a>
The Amazon Resource Name \(ARN\) of the work team for which you want to see labeling jobs for\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:workteam/.*`   
Required: Yes

## Response Syntax<a name="API_ListLabelingJobsForWorkteam_ResponseSyntax"></a>

```
{
   "[LabelingJobSummaryList](#SageMaker-ListLabelingJobsForWorkteam-response-LabelingJobSummaryList)": [ 
      { 
         "[CreationTime](API_LabelingJobForWorkteamSummary.md#SageMaker-Type-LabelingJobForWorkteamSummary-CreationTime)": number,
         "[JobReferenceCode](API_LabelingJobForWorkteamSummary.md#SageMaker-Type-LabelingJobForWorkteamSummary-JobReferenceCode)": "string",
         "[LabelCounters](API_LabelingJobForWorkteamSummary.md#SageMaker-Type-LabelingJobForWorkteamSummary-LabelCounters)": { 
            "[HumanLabeled](API_LabelCountersForWorkteam.md#SageMaker-Type-LabelCountersForWorkteam-HumanLabeled)": number,
            "[PendingHuman](API_LabelCountersForWorkteam.md#SageMaker-Type-LabelCountersForWorkteam-PendingHuman)": number,
            "[Total](API_LabelCountersForWorkteam.md#SageMaker-Type-LabelCountersForWorkteam-Total)": number
         },
         "[LabelingJobName](API_LabelingJobForWorkteamSummary.md#SageMaker-Type-LabelingJobForWorkteamSummary-LabelingJobName)": "string",
         "[WorkRequesterAccountId](API_LabelingJobForWorkteamSummary.md#SageMaker-Type-LabelingJobForWorkteamSummary-WorkRequesterAccountId)": "string"
      }
   ],
   "[NextToken](#SageMaker-ListLabelingJobsForWorkteam-response-NextToken)": "string"
}
```

## Response Elements<a name="API_ListLabelingJobsForWorkteam_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [LabelingJobSummaryList](#API_ListLabelingJobsForWorkteam_ResponseSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-response-LabelingJobSummaryList"></a>
An array of `LabelingJobSummary` objects, each describing a labeling job\.  
Type: Array of [LabelingJobForWorkteamSummary](API_LabelingJobForWorkteamSummary.md) objects

 ** [NextToken](#API_ListLabelingJobsForWorkteam_ResponseSyntax) **   <a name="SageMaker-ListLabelingJobsForWorkteam-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of labeling jobs, use it in the subsequent request\.  
Type: String  
Length Constraints: Maximum length of 8192\.

## Errors<a name="API_ListLabelingJobsForWorkteam_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_ListLabelingJobsForWorkteam_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListLabelingJobsForWorkteam) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListLabelingJobsForWorkteam) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListLabelingJobsForWorkteam) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListLabelingJobsForWorkteam) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListLabelingJobsForWorkteam) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListLabelingJobsForWorkteam) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListLabelingJobsForWorkteam) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListLabelingJobsForWorkteam) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListLabelingJobsForWorkteam) 