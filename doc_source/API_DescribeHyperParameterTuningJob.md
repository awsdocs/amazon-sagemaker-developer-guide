# DescribeHyperParameterTuningJob<a name="API_DescribeHyperParameterTuningJob"></a>

Gets a description of a hyperparameter tuning job\.

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
The name of the tuning job to describe\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeHyperParameterTuningJob_ResponseSyntax"></a>

```
{
   "[BestTrainingJob](#SageMaker-DescribeHyperParameterTuningJob-response-BestTrainingJob)": { 
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
               "[Name](API_ContinuousParameterRange.md#SageMaker-Type-ContinuousParameterRange-Name)": "string",
               "[ScalingType](API_ContinuousParameterRange.md#SageMaker-Type-ContinuousParameterRange-ScalingType)": "string"
            }
         ],
         "[IntegerParameterRanges](API_ParameterRanges.md#SageMaker-Type-ParameterRanges-IntegerParameterRanges)": [ 
            { 
               "[MaxValue](API_IntegerParameterRange.md#SageMaker-Type-IntegerParameterRange-MaxValue)": "string",
               "[MinValue](API_IntegerParameterRange.md#SageMaker-Type-IntegerParameterRange-MinValue)": "string",
               "[Name](API_IntegerParameterRange.md#SageMaker-Type-IntegerParameterRange-Name)": "string",
               "[ScalingType](API_IntegerParameterRange.md#SageMaker-Type-IntegerParameterRange-ScalingType)": "string"
            }
         ]
      },
      "[ResourceLimits](API_HyperParameterTuningJobConfig.md#SageMaker-Type-HyperParameterTuningJobConfig-ResourceLimits)": { 
         "[MaxNumberOfTrainingJobs](API_ResourceLimits.md#SageMaker-Type-ResourceLimits-MaxNumberOfTrainingJobs)": number,
         "[MaxParallelTrainingJobs](API_ResourceLimits.md#SageMaker-Type-ResourceLimits-MaxParallelTrainingJobs)": number
      },
      "[Strategy](API_HyperParameterTuningJobConfig.md#SageMaker-Type-HyperParameterTuningJobConfig-Strategy)": "string",
      "[TrainingJobEarlyStoppingType](API_HyperParameterTuningJobConfig.md#SageMaker-Type-HyperParameterTuningJobConfig-TrainingJobEarlyStoppingType)": "string"
   },
   "[HyperParameterTuningJobName](#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobName)": "string",
   "[HyperParameterTuningJobStatus](#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus)": "string",
   "[LastModifiedTime](#SageMaker-DescribeHyperParameterTuningJob-response-LastModifiedTime)": number,
   "[ObjectiveStatusCounters](#SageMaker-DescribeHyperParameterTuningJob-response-ObjectiveStatusCounters)": { 
      "[Failed](API_ObjectiveStatusCounters.md#SageMaker-Type-ObjectiveStatusCounters-Failed)": number,
      "[Pending](API_ObjectiveStatusCounters.md#SageMaker-Type-ObjectiveStatusCounters-Pending)": number,
      "[Succeeded](API_ObjectiveStatusCounters.md#SageMaker-Type-ObjectiveStatusCounters-Succeeded)": number
   },
   "[OverallBestTrainingJob](#SageMaker-DescribeHyperParameterTuningJob-response-OverallBestTrainingJob)": { 
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
   },
   "[TrainingJobDefinition](#SageMaker-DescribeHyperParameterTuningJob-response-TrainingJobDefinition)": { 
      "[AlgorithmSpecification](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-AlgorithmSpecification)": { 
         "[AlgorithmName](API_HyperParameterAlgorithmSpecification.md#SageMaker-Type-HyperParameterAlgorithmSpecification-AlgorithmName)": "string",
         "[MetricDefinitions](API_HyperParameterAlgorithmSpecification.md#SageMaker-Type-HyperParameterAlgorithmSpecification-MetricDefinitions)": [ 
            { 
               "[Name](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Name)": "string",
               "[Regex](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Regex)": "string"
            }
         ],
         "[TrainingImage](API_HyperParameterAlgorithmSpecification.md#SageMaker-Type-HyperParameterAlgorithmSpecification-TrainingImage)": "string",
         "[TrainingInputMode](API_HyperParameterAlgorithmSpecification.md#SageMaker-Type-HyperParameterAlgorithmSpecification-TrainingInputMode)": "string"
      },
      "[EnableInterContainerTrafficEncryption](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-EnableInterContainerTrafficEncryption)": boolean,
      "[EnableNetworkIsolation](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-EnableNetworkIsolation)": boolean,
      "[InputDataConfig](API_HyperParameterTrainingJobDefinition.md#SageMaker-Type-HyperParameterTrainingJobDefinition-InputDataConfig)": [ 
         { 
            "[ChannelName](API_Channel.md#SageMaker-Type-Channel-ChannelName)": "string",
            "[CompressionType](API_Channel.md#SageMaker-Type-Channel-CompressionType)": "string",
            "[ContentType](API_Channel.md#SageMaker-Type-Channel-ContentType)": "string",
            "[DataSource](API_Channel.md#SageMaker-Type-Channel-DataSource)": { 
               "[S3DataSource](API_DataSource.md#SageMaker-Type-DataSource-S3DataSource)": { 
                  "[AttributeNames](API_S3DataSource.md#SageMaker-Type-S3DataSource-AttributeNames)": [ "string" ],
                  "[S3DataDistributionType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataDistributionType)": "string",
                  "[S3DataType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataType)": "string",
                  "[S3Uri](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3Uri)": "string"
               }
            },
            "[InputMode](API_Channel.md#SageMaker-Type-Channel-InputMode)": "string",
            "[RecordWrapperType](API_Channel.md#SageMaker-Type-Channel-RecordWrapperType)": "string",
            "[ShuffleConfig](API_Channel.md#SageMaker-Type-Channel-ShuffleConfig)": { 
               "[Seed](API_ShuffleConfig.md#SageMaker-Type-ShuffleConfig-Seed)": number
            }
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
   },
   "[TrainingJobStatusCounters](#SageMaker-DescribeHyperParameterTuningJob-response-TrainingJobStatusCounters)": { 
      "[Completed](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-Completed)": number,
      "[InProgress](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-InProgress)": number,
      "[NonRetryableError](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-NonRetryableError)": number,
      "[RetryableError](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-RetryableError)": number,
      "[Stopped](API_TrainingJobStatusCounters.md#SageMaker-Type-TrainingJobStatusCounters-Stopped)": number
   },
   "[WarmStartConfig](#SageMaker-DescribeHyperParameterTuningJob-response-WarmStartConfig)": { 
      "[ParentHyperParameterTuningJobs](API_HyperParameterTuningJobWarmStartConfig.md#SageMaker-Type-HyperParameterTuningJobWarmStartConfig-ParentHyperParameterTuningJobs)": [ 
         { 
            "[HyperParameterTuningJobName](API_ParentHyperParameterTuningJob.md#SageMaker-Type-ParentHyperParameterTuningJob-HyperParameterTuningJobName)": "string"
         }
      ],
      "[WarmStartType](API_HyperParameterTuningJobWarmStartConfig.md#SageMaker-Type-HyperParameterTuningJobWarmStartConfig-WarmStartType)": "string"
   }
}
```

## Response Elements<a name="API_DescribeHyperParameterTuningJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [BestTrainingJob](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-BestTrainingJob"></a>
A [TrainingJobSummary](API_TrainingJobSummary.md) object that describes the training job that completed with the best current [HyperParameterTuningJobObjective](API_HyperParameterTuningJobObjective.md)\.  
Type: [HyperParameterTrainingJobSummary](API_HyperParameterTrainingJobSummary.md) object

 ** [CreationTime](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-CreationTime"></a>
The date and time that the tuning job started\.  
Type: Timestamp

 ** [FailureReason](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-FailureReason"></a>
If the tuning job failed, the reason it failed\.  
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [HyperParameterTuningEndTime](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningEndTime"></a>
The date and time that the tuning job ended\.  
Type: Timestamp

 ** [HyperParameterTuningJobArn](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobArn"></a>
The Amazon Resource Name \(ARN\) of the tuning job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:hyper-parameter-tuning-job/.*` 

 ** [HyperParameterTuningJobConfig](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobConfig"></a>
The [HyperParameterTuningJobConfig](API_HyperParameterTuningJobConfig.md) object that specifies the configuration of the tuning job\.  
Type: [HyperParameterTuningJobConfig](API_HyperParameterTuningJobConfig.md) object

 ** [HyperParameterTuningJobName](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobName"></a>
The name of the tuning job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [HyperParameterTuningJobStatus](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus"></a>
The status of the tuning job: InProgress, Completed, Failed, Stopping, or Stopped\.  
Type: String  
Valid Values:` Completed | InProgress | Failed | Stopped | Stopping` 

 ** [LastModifiedTime](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-LastModifiedTime"></a>
The date and time that the status of the tuning job was modified\.   
Type: Timestamp

 ** [ObjectiveStatusCounters](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-ObjectiveStatusCounters"></a>
The [ObjectiveStatusCounters](API_ObjectiveStatusCounters.md) object that specifies the number of training jobs, categorized by the status of their final objective metric, that this tuning job launched\.  
Type: [ObjectiveStatusCounters](API_ObjectiveStatusCounters.md) object

 ** [OverallBestTrainingJob](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-OverallBestTrainingJob"></a>
If the hyperparameter tuning job is an warm start tuning job with a `WarmStartType` of `IDENTICAL_DATA_AND_ALGORITHM`, this is the [TrainingJobSummary](API_TrainingJobSummary.md) for the training job with the best objective metric value of all training jobs launched by this tuning job and all parent jobs specified for the warm start tuning job\.  
Type: [HyperParameterTrainingJobSummary](API_HyperParameterTrainingJobSummary.md) object

 ** [TrainingJobDefinition](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-TrainingJobDefinition"></a>
The [HyperParameterTrainingJobDefinition](API_HyperParameterTrainingJobDefinition.md) object that specifies the definition of the training jobs that this tuning job launches\.  
Type: [HyperParameterTrainingJobDefinition](API_HyperParameterTrainingJobDefinition.md) object

 ** [TrainingJobStatusCounters](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-TrainingJobStatusCounters"></a>
The [TrainingJobStatusCounters](API_TrainingJobStatusCounters.md) object that specifies the number of training jobs, categorized by status, that this tuning job launched\.  
Type: [TrainingJobStatusCounters](API_TrainingJobStatusCounters.md) object

 ** [WarmStartConfig](#API_DescribeHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-DescribeHyperParameterTuningJob-response-WarmStartConfig"></a>
The configuration for starting the hyperparameter parameter tuning job using one or more previous tuning jobs as a starting point\. The results of previous tuning jobs are used to inform which combinations of hyperparameters to search over in the new tuning job\.  
Type: [HyperParameterTuningJobWarmStartConfig](API_HyperParameterTuningJobWarmStartConfig.md) object

## Errors<a name="API_DescribeHyperParameterTuningJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeHyperParameterTuningJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeHyperParameterTuningJob) 