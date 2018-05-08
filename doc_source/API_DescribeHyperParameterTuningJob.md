# DescribeHyperParameterTuningJob<a name="API_DescribeHyperParameterTuningJob"></a>

## Request Syntax<a name="API_DescribeHyperParameterTuningJob_RequestSyntax"></a>

```
{
   "[HyperParameterTuningJobName](#SageMaker-DescribeHyperParameterTuningJob-request-HyperParameterTuningJobName)": "string"
}
```

## Request Parameters<a name="API_DescribeHyperParameterTuningJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [HyperParameterTuningJobName](#API_DescribeHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-request-HyperParameterTuningJobName"></a>
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeHyperParameterTuningJob_ResponseSyntax"></a>

```
{
   "[BestTrainingJob](#SageMaker-DescribeHyperParameterTuningJob-response-BestTrainingJob)": { 
      "[CreationTime](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-CreationTime)": number,
      "[FinalHyperParameterTuningJobObjectiveMetric](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-FinalHyperParameterTuningJobObjectiveMetric)": { 
         "[MetricName](API_FinalHyperParameterTuningJobObjectiveMetric.md#SageMaker-Type-FinalHyperParameterTuningJobObjectiveMetric-MetricName)": "string",
         "[Type](API_FinalHyperParameterTuningJobObjectiveMetric.md#SageMaker-Type-FinalHyperParameterTuningJobObjectiveMetric-Type)": "string",
         "[Value](API_FinalHyperParameterTuningJobObjectiveMetric.md#SageMaker-Type-FinalHyperParameterTuningJobObjectiveMetric-Value)": number
      },
      "[TrainingEndTime](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TrainingEndTime)": number,
      "[TrainingJobArn](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobArn)": "string",
      "[TrainingJobName](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobName)": "string",
      "[TrainingJobStatus](API_HyperParameterTrainingJobSummary.md#SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobStatus)": "string"
   },
   "[CreationTime](#SageMaker-DescribeHyperParameterTuningJob-response-CreationTime)": number,
   "[FailureReason](#SageMaker-DescribeHyperParameterTuningJob-response-FailureReason)": "string",
   "[HyperParameterTuningEndTime](#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningEndTime)": number,
   "[HyperParameterTuningJobArn](#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobArn)": "string",
   "[HyperParameterTuningJobConfig](#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobConfig)": { 
      "[HyperParameterTuningJobObjective](API_HyperParameterTuningJobConfig.md#SageMaker-Type-HyperParameterTuningJobConfig-HyperParameterTuningJobObjective)": { 
         "[MetricName](API_HyperParameterTuningJobObjective.md#SageMaker-Type-HyperParameterTuningJobObjective-MetricName)": "string",
         "[Type](API_HyperParameterTuningJobObjective.md#SageMaker-Type-HyperParameterTuningJobObjective-Type)": "string"
      },
      "[ParameterRanges](API_HyperParameterTuningJobConfig.md#SageMaker-Type-HyperParameterTuningJobConfig-ParameterRanges)": { 
         "[CategoricalParameterRanges](API_ParameterRanges.md#SageMaker-Type-ParameterRanges-CategoricalParameterRanges)": [ 
            { 
               "[Name](API_CategoricalParameterRange.md#SageMaker-Type-CategoricalParameterRange-Name)": "string",
               "[Values](API_CategoricalParameterRange.md#SageMaker-Type-CategoricalParameterRange-Values)": [ "string" ]
            }
         ],
         "[ContinuousParameterRanges](API_ParameterRanges.md#SageMaker-Type-ParameterRanges-ContinuousParameterRanges)": [ 
            { 
               "[MaxValue](API_ContinuousParameterRange.md#SageMaker-Type-ContinuousParameterRange-MaxValue)": "string",
               "[MinValue](API_ContinuousParameterRange.md#SageMaker-Type-ContinuousParameterRange-MinValue)": "string",
               "[Name](API_ContinuousParameterRange.md#SageMaker-Type-ContinuousParameterRange-Name)": "string"
            }
         ],
         "[IntegerParameterRanges](API_ParameterRanges.md#SageMaker-Type-ParameterRanges-IntegerParameterRanges)": [ 
            { 
               "[MaxValue](API_IntegerParameterRange.md#SageMaker-Type-IntegerParameterRange-MaxValue)": "string",
               "[MinValue](API_IntegerParameterRange.md#SageMaker-Type-IntegerParameterRange-MinValue)": "string",
               "[Name](API_IntegerParameterRange.md#SageMaker-Type-IntegerParameterRange-Name)": "string"
            }
         ]
      },
      "[ResourceLimits](API_HyperParameterTuningJobConfig.md#SageMaker-Type-HyperParameterTuningJobConfig-ResourceLimits)": { 
         "[MaxNumberOfTrainingJobs](API_ResourceLimits.md#SageMaker-Type-ResourceLimits-MaxNumberOfTrainingJobs)": number,
         "[MaxParallelTrainingJobs](API_ResourceLimits.md#SageMaker-Type-ResourceLimits-MaxParallelTrainingJobs)": number
      },
      "[Strategy](API_HyperParameterTuningJobConfig.md#SageMaker-Type-HyperParameterTuningJobConfig-Strategy)": "string"
   },
   "[HyperParameterTuningJobName](#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobName)": "string",
   "[HyperParameterTuningJobStatus](#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus)": "string",
   "[LastModifiedTime](#SageMaker-DescribeHyperParameterTuningJob-response-LastModifiedTime)": number,
   "[TrainingJobCounters](#SageMaker-DescribeHyperParameterTuningJob-response-TrainingJobCounters)": { 
      "[ClientError](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-ClientError)": number,
      "[Completed](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-Completed)": number,
      "[Fault](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-Fault)": number,
      "[InProgress](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-InProgress)": number,
      "[Stopped](API_TrainingJobCounters.md#SageMaker-Type-TrainingJobCounters-Stopped)": number
   },
   "[TrainingJobDefinition](#SageMaker-DescribeHyperParameterTuningJob-response-TrainingJobDefinition)": { 
      "[AlgorithmSpecification](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-AlgorithmSpecification)": { 
         "[MetricDefinitions](API_HyperParameterAlgorithmSpecification.md#SageMaker-Type-HyperParameterAlgorithmSpecification-MetricDefinitions)": [ 
            { 
               "[Name](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Name)": "string",
               "[Regex](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Regex)": "string"
            }
         ],
         "[TrainingImage](API_HyperParameterAlgorithmSpecification.md#SageMaker-Type-HyperParameterAlgorithmSpecification-TrainingImage)": "string",
         "[TrainingInputMode](API_HyperParameterAlgorithmSpecification.md#SageMaker-Type-HyperParameterAlgorithmSpecification-TrainingInputMode)": "string"
      },
      "[InputDataConfig](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-InputDataConfig)": [ 
         { 
            "[ChannelName](API_Channel.md#SageMaker-Type-Channel-ChannelName)": "string",
            "[CompressionType](API_Channel.md#SageMaker-Type-Channel-CompressionType)": "string",
            "[ContentType](API_Channel.md#SageMaker-Type-Channel-ContentType)": "string",
            "[DataSource](API_Channel.md#SageMaker-Type-Channel-DataSource)": { 
               "[S3DataSource](API_DataSource.md#SageMaker-Type-DataSource-S3DataSource)": { 
                  "[S3DataDistributionType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataDistributionType)": "string",
                  "[S3DataType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataType)": "string",
                  "[S3Uri](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3Uri)": "string"
               }
            },
            "[RecordWrapperType](API_Channel.md#SageMaker-Type-Channel-RecordWrapperType)": "string"
         }
      ],
      "[OutputDataConfig](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-OutputDataConfig)": { 
         "[KmsKeyId](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-KmsKeyId)": "string",
         "[S3OutputPath](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-S3OutputPath)": "string"
      },
      "[ResourceConfig](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-ResourceConfig)": { 
         "[InstanceCount](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceCount)": number,
         "[InstanceType](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceType)": "string",
         "[VolumeKmsKeyId](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeKmsKeyId)": "string",
         "[VolumeSizeInGB](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeSizeInGB)": number
      },
      "[RoleArn](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-RoleArn)": "string",
      "[StaticHyperParameters](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-StaticHyperParameters)": { 
         "string" : "string" 
      },
      "[StoppingCondition](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-StoppingCondition)": { 
         "[MaxRuntimeInSeconds](API_StoppingCondition.md#SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds)": number
      },
      "[VpcConfig](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-VpcConfig)": { 
         "[SecurityGroupIds](API_VpcConfig.md#SageMaker-Type-VpcConfig-SecurityGroupIds)": [ "string" ],
         "[Subnets](API_VpcConfig.md#SageMaker-Type-VpcConfig-Subnets)": [ "string" ]
      }
   }
}
```

## Response Elements<a name="API_DescribeHyperParameterTuningJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [BestTrainingJob](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-BestTrainingJob"></a>
Type: [HyperParameterTrainingJobSummary](API_HyperParameterTrainingJobSummary.md) object

 ** [CreationTime](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-CreationTime"></a>
Type: Timestamp

 ** [FailureReason](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-FailureReason"></a>
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [HyperParameterTuningEndTime](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningEndTime"></a>
Type: Timestamp

 ** [HyperParameterTuningJobArn](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobArn"></a>
Type: String  
Length Constraints: Maximum length of 256\.

 ** [HyperParameterTuningJobConfig](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobConfig"></a>
Type: [HyperParameterTuningJobConfig](API_HyperParameterTuningJobConfig.md) object

 ** [HyperParameterTuningJobName](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobName"></a>
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [HyperParameterTuningJobStatus](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus"></a>
Type: String  
Valid Values:` Completed | InProgress | Failed | Stopped | Stopping` 

 ** [LastModifiedTime](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-LastModifiedTime"></a>
Type: Timestamp

 ** [TrainingJobCounters](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-TrainingJobCounters"></a>
Type: [TrainingJobCounters](API_TrainingJobCounters.md) object

 ** [TrainingJobDefinition](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-TrainingJobDefinition"></a>
Type: [HyperParameterTrainingJobDefinition](API_HyperParameterTrainingJobDefinition.md) object

## Errors<a name="API_DescribeHyperParameterTuningJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeHyperParameterTuningJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 