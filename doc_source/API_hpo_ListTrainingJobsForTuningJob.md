# ListTrainingJobsForTuningJob<a name="API_hpo_ListTrainingJobsForTuningJob"></a>

## Request Syntax<a name="API_hpo_ListTrainingJobsForTuningJob_RequestSyntax"></a>

```
POST /ListTrainingJobsForTuningJob HTTP/1.1
Content-type: application/json

{
   "AwsAccountId": "string",
   "MaxResults": number,
   "NextToken": "string",
   "SortBy": "string",
   "SortOrder": "string",
   "TrainingJobStatusEquals": "string",
   "TuningJobName": "string"
}
```

## URI Request Parameters<a name="API_hpo_ListTrainingJobsForTuningJob_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_hpo_ListTrainingJobsForTuningJob_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** AwsAccountId **   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 12\.  
Pattern: `[0-9]{12}`   
Required: Yes

 ** MaxResults **   
Type: Integer  
Valid Range: Maximum value of 100\.  
Required: No

 ** NextToken **   
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** SortBy **   
Type: String  
Valid Values:` FinalObjectiveMetricValue`   
Required: No

 ** SortOrder **   
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

 ** TrainingJobStatusEquals **   
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: No

 ** TuningJobName **   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_hpo_ListTrainingJobsForTuningJob_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "NextToken": "string",
   "TrainingJobSummaries": [ 
      { 
         "FinalTuningJobObjectiveMetric": { 
            "MetricName": "string",
            "Type": "string",
            "Value": number
         },
         "HyperParameters": { 
            "string" : "string" 
         },
         "RetriedAttempt": number,
         "TrainingJobName": "string",
         "TrainingJobStatus": "string"
      }
   ]
}
```

## Response Elements<a name="API_hpo_ListTrainingJobsForTuningJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** NextToken **   
Type: String  
Length Constraints: Maximum length of 8192\.

 ** TrainingJobSummaries **   
Type: Array of [TrainingJobSummary](API_hpo_TrainingJobSummary.md) objects

## Errors<a name="API_hpo_ListTrainingJobsForTuningJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **AccessForbidden**   
HTTP Status Code: 403

 **InternalFailure**   
HTTP Status Code: 500

 **ResourceNotFound**   
HTTP Status Code: 404

 **ServiceUnavailable**   
HTTP Status Code: 503

 **Throttling**   
HTTP Status Code: 401

 **ValidationError**   
HTTP Status Code: 400

## See Also<a name="API_hpo_ListTrainingJobsForTuningJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemakerhpo-2017-11-08/ListTrainingJobsForTuningJob) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemakerhpo-2017-11-08/ListTrainingJobsForTuningJob) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemakerhpo-2017-11-08/ListTrainingJobsForTuningJob) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemakerhpo-2017-11-08/ListTrainingJobsForTuningJob) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemakerhpo-2017-11-08/ListTrainingJobsForTuningJob) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemakerhpo-2017-11-08/ListTrainingJobsForTuningJob) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemakerhpo-2017-11-08/ListTrainingJobsForTuningJob) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemakerhpo-2017-11-08/ListTrainingJobsForTuningJob) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemakerhpo-2017-11-08/ListTrainingJobsForTuningJob) 