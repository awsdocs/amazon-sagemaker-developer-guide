# ListTransformJobs<a name="API_ListTransformJobs"></a>

## Request Syntax<a name="API_ListTransformJobs_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListTransformJobs-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListTransformJobs-request-CreationTimeBefore)": number,
   "[LastModifiedTimeAfter](#SageMaker-ListTransformJobs-request-LastModifiedTimeAfter)": number,
   "[LastModifiedTimeBefore](#SageMaker-ListTransformJobs-request-LastModifiedTimeBefore)": number,
   "[MaxResults](#SageMaker-ListTransformJobs-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListTransformJobs-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListTransformJobs-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListTransformJobs-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListTransformJobs-request-SortOrder)": "string",
   "[StatusEquals](#SageMaker-ListTransformJobs-request-StatusEquals)": "string"
}
```

## Request Parameters<a name="API_ListTransformJobs_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-CreationTimeAfter"></a>
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-CreationTimeBefore"></a>
Type: Timestamp  
Required: No

 ** [LastModifiedTimeAfter](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-LastModifiedTimeAfter"></a>
Type: Timestamp  
Required: No

 ** [LastModifiedTimeBefore](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-LastModifiedTimeBefore"></a>
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-MaxResults"></a>
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-NameContains"></a>
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9\-]+`   
Required: No

 ** [NextToken](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-NextToken"></a>
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [SortBy](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-SortBy"></a>
Type: String  
Valid Values:` Name | CreationTime | Status`   
Required: No

 ** [SortOrder](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-SortOrder"></a>
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** [StatusEquals](#API_ListTransformJobs_RequestSyntax) **   <a name="SageMaker-ListTransformJobs-request-StatusEquals"></a>
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: No

## Response Syntax<a name="API_ListTransformJobs_ResponseSyntax"></a>

```
{
   "[NextToken](#SageMaker-ListTransformJobs-response-NextToken)": "string",
   "[TransformJobSummaries](#SageMaker-ListTransformJobs-response-TransformJobSummaries)": [ 
      { 
         "[CreationTime](API_TransformJobSummary.md#SageMaker-Type-TransformJobSummary-CreationTime)": number,
         "[FailureReason](API_TransformJobSummary.md#SageMaker-Type-TransformJobSummary-FailureReason)": "string",
         "[LastModifiedTime](API_TransformJobSummary.md#SageMaker-Type-TransformJobSummary-LastModifiedTime)": number,
         "[TransformEndTime](API_TransformJobSummary.md#SageMaker-Type-TransformJobSummary-TransformEndTime)": number,
         "[TransformJobArn](API_TransformJobSummary.md#SageMaker-Type-TransformJobSummary-TransformJobArn)": "string",
         "[TransformJobName](API_TransformJobSummary.md#SageMaker-Type-TransformJobSummary-TransformJobName)": "string",
         "[TransformJobStatus](API_TransformJobSummary.md#SageMaker-Type-TransformJobSummary-TransformJobStatus)": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListTransformJobs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NextToken](#API_ListTransformJobs_ResponseSyntax) **   <a name="SageMaker-ListTransformJobs-response-NextToken"></a>
Type: String  
Length Constraints: Maximum length of 8192\.

 ** [TransformJobSummaries](#API_ListTransformJobs_ResponseSyntax) **   <a name="SageMaker-ListTransformJobs-response-TransformJobSummaries"></a>
Type: Array of [TransformJobSummary](API_TransformJobSummary.md) objects

## Errors<a name="API_ListTransformJobs_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListTransformJobs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListTransformJobs) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListTransformJobs) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListTransformJobs) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListTransformJobs) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListTransformJobs) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListTransformJobs) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListTransformJobs) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListTransformJobs) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListTransformJobs) 