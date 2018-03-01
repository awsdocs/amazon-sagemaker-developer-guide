# DescribeTuningJob<a name="API_hpo_DescribeTuningJob"></a>

## Request Syntax<a name="API_hpo_DescribeTuningJob_RequestSyntax"></a>

```
POST /DescribeTuningJob HTTP/1.1
Content-type: application/json

{
   "AwsAccountId": "string",
   "TuningJobName": "string"
}
```

## URI Request Parameters<a name="API_hpo_DescribeTuningJob_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_hpo_DescribeTuningJob_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** AwsAccountId **   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 12\.  
Pattern: `[0-9]{12}`   
Required: Yes

 ** TuningJobName **   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_hpo_DescribeTuningJob_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "BestTrainingJob": { 
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
   },
   "FailureReason": "string",
   "MetricDefinitions": [ 
      { 
         "Name": "string",
         "Regex": "string"
      }
   ],
   "ParameterRanges": { 
      "CategoricalParameterRanges": [ 
         { 
            "Name": "string",
            "Type": "string",
            "Values": [ "string" ]
         }
      ],
      "ContinuousParameterRanges": [ 
         { 
            "MaxValue": "string",
            "MinValue": "string",
            "Name": "string",
            "Type": "string"
         }
      ],
      "IntegerParameterRanges": [ 
         { 
            "MaxValue": "string",
            "MinValue": "string",
            "Name": "string",
            "Type": "string"
         }
      ]
   },
   "ResourcesStats": { 
      "ConsumedResources": { 
         "ComputeHours": { 
            "Finished": number,
            "InFlight": number
         },
         "Jobs": { 
            "Completed": number,
            "Failed": number,
            "Pending": number,
            "Running": number
         }
      },
      "ResourceLimits": { 
         "MaxClockTimeInHours": number,
         "MaxNumberOfTrainingJobs": number,
         "MaxParallelTrainingJobs": number
      }
   },
   "Strategy": "string",
   "TuningJobEndTime": number,
   "TuningJobName": "string",
   "TuningJobStartTime": number,
   "TuningJobStatus": "string"
}
```

## Response Elements<a name="API_hpo_DescribeTuningJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** BestTrainingJob **   
Type: [TrainingJobSummary](API_hpo_TrainingJobSummary.md) object

 ** FailureReason **   
Type: String  
Length Constraints: Maximum length of 1024\.

 ** MetricDefinitions **   
Type: Array of [MetricDefinition](API_hpo_MetricDefinition.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 20 items\.

 ** ParameterRanges **   
Type: [ParameterRanges](API_hpo_ParameterRanges.md) object

 ** ResourcesStats **   
Type: [ResourcesStats](API_hpo_ResourcesStats.md) object

 ** Strategy **   
Type: String  
Valid Values:` Bayesian` 

 ** TuningJobEndTime **   
Type: Timestamp

 ** TuningJobName **   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** TuningJobStartTime **   
Type: Timestamp

 ** TuningJobStatus **   
Type: String  
Valid Values:` Created | Completed | Running | Failed | Stopped | Stopping | WaitingForParameters` 

## Errors<a name="API_hpo_DescribeTuningJob_Errors"></a>

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

## See Also<a name="API_hpo_DescribeTuningJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemakerhpo-2017-11-08/DescribeTuningJob) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemakerhpo-2017-11-08/DescribeTuningJob) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemakerhpo-2017-11-08/DescribeTuningJob) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemakerhpo-2017-11-08/DescribeTuningJob) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemakerhpo-2017-11-08/DescribeTuningJob) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemakerhpo-2017-11-08/DescribeTuningJob) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemakerhpo-2017-11-08/DescribeTuningJob) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemakerhpo-2017-11-08/DescribeTuningJob) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemakerhpo-2017-11-08/DescribeTuningJob) 