# CreateTuningJob<a name="API_hpo_CreateTuningJob"></a>

## Request Syntax<a name="API_hpo_CreateTuningJob_RequestSyntax"></a>

```
POST /CreateTuningJob HTTP/1.1
Content-type: application/json

{
   "AwsAccountId": "string",
   "Tags": [ 
      { 
         "Key": "string",
         "Value": "string"
      }
   ],
   "TrainingJobDefinition": { 
      "AlgorithmSpecification": { 
         "MetricDefinitions": [ 
            { 
               "Name": "string",
               "Regex": "string"
            }
         ],
         "TrainingImage": "string",
         "TrainingInputMode": "string"
      },
      "InputDataConfig": [ 
         { 
            "ChannelName": "string",
            "CompressionType": "string",
            "ContentType": "string",
            "DataSource": { 
               "S3DataSource": { 
                  "S3DataDistributionType": "string",
                  "S3DataType": "string",
                  "S3Uri": "string"
               }
            },
            "RecordWrapperType": "string"
         }
      ],
      "OutputDataConfig": { 
         "KmsKeyId": "string",
         "S3OutputPath": "string"
      },
      "ResourceConfig": { 
         "InstanceCount": number,
         "InstanceType": "string",
         "VolumeKmsKeyId": "string",
         "VolumeSizeInGB": number
      },
      "RoleArn": "string",
      "StaticHyperParameters": { 
         "string" : "string" 
      },
      "StoppingCondition": { 
         "MaxRuntimeInSeconds": number
      },
      "Tags": [ 
         { 
            "Key": "string",
            "Value": "string"
         }
      ]
   },
   "TuningJobConfig": { 
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
      "ResourceLimits": { 
         "MaxClockTimeInHours": number,
         "MaxNumberOfTrainingJobs": number,
         "MaxParallelTrainingJobs": number
      },
      "Strategy": "string",
      "TuningJobObjective": { 
         "MetricName": "string",
         "Type": "string"
      }
   },
   "TuningJobName": "string"
}
```

## URI Request Parameters<a name="API_hpo_CreateTuningJob_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_hpo_CreateTuningJob_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** AwsAccountId **   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 12\.  
Pattern: `[0-9]{12}`   
Required: Yes

 ** Tags **   
Type: Array of [Tag](API_hpo_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

 ** TrainingJobDefinition **   
Type: [TrainingJobDefinition](API_hpo_TrainingJobDefinition.md) object  
Required: Yes

 ** TuningJobConfig **   
Type: [TuningJobConfig](API_hpo_TuningJobConfig.md) object  
Required: Yes

 ** TuningJobName **   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_hpo_CreateTuningJob_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "TuningJobArn": "string"
}
```

## Response Elements<a name="API_hpo_CreateTuningJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** TuningJobArn **   
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws:sagemaker:[\p{Alnum}\-]*:[0-9]{12}:tuning-job/.*` 

## Errors<a name="API_hpo_CreateTuningJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **AccessForbidden**   
HTTP Status Code: 403

 **InternalFailure**   
HTTP Status Code: 500

 **ResourceInUse**   
HTTP Status Code: 400

 **ServiceUnavailable**   
HTTP Status Code: 503

 **Throttling**   
HTTP Status Code: 401

 **ValidationError**   
HTTP Status Code: 400

## See Also<a name="API_hpo_CreateTuningJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemakerhpo-2017-11-08/CreateTuningJob) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemakerhpo-2017-11-08/CreateTuningJob) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemakerhpo-2017-11-08/CreateTuningJob) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemakerhpo-2017-11-08/CreateTuningJob) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemakerhpo-2017-11-08/CreateTuningJob) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemakerhpo-2017-11-08/CreateTuningJob) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemakerhpo-2017-11-08/CreateTuningJob) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemakerhpo-2017-11-08/CreateTuningJob) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemakerhpo-2017-11-08/CreateTuningJob) 