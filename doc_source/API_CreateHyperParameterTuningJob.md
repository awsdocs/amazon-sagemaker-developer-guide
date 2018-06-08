# CreateHyperParameterTuningJob<a name="API_CreateHyperParameterTuningJob"></a>

Starts a hyperparameter tuning job\.

## Request Syntax<a name="API_CreateHyperParameterTuningJob_RequestSyntax"></a>

```
{
   "[HyperParameterTuningJobConfig](#SageMaker-CreateHyperParameterTuningJob-request-HyperParameterTuningJobConfig)": { 
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
   "[HyperParameterTuningJobName](#SageMaker-CreateHyperParameterTuningJob-request-HyperParameterTuningJobName)": "string",
   "[Tags](#SageMaker-CreateHyperParameterTuningJob-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ],
   "[TrainingJobDefinition](#SageMaker-CreateHyperParameterTuningJob-request-TrainingJobDefinition)": { 
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

## Request Parameters<a name="API_CreateHyperParameterTuningJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [HyperParameterTuningJobConfig](#API_CreateHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-CreateHyperParameterTuningJob-request-HyperParameterTuningJobConfig"></a>
The [HyperParameterTuningJobConfig](API_HyperParameterTuningJobConfig.md) object that describes the tuning job, including the search strategy, metric used to evaluate training jobs, ranges of parameters to search, and resource limits for the tuning job\.  
Type: [HyperParameterTuningJobConfig](API_HyperParameterTuningJobConfig.md) object  
Required: Yes

 ** [HyperParameterTuningJobName](#API_CreateHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-CreateHyperParameterTuningJob-request-HyperParameterTuningJobName"></a>
The name of the tuning job\. This name is the prefix for the names of all training jobs that this tuning job launches\. The name must be unique within the same AWS account and AWS Region\. Names are not case sensitive, and must be between 1\-32 characters\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [Tags](#API_CreateHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-CreateHyperParameterTuningJob-request-Tags"></a>
An array of key\-value pairs\. You can use tags to categorize your AWS resources in different ways, for example, by purpose, owner, or environment\. For more information, see [Using Cost Allocation Tags](http://docs.aws.amazon.com//awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

 ** [TrainingJobDefinition](#API_CreateHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-CreateHyperParameterTuningJob-request-TrainingJobDefinition"></a>
The [HyperParameterTrainingJobDefinition](API_HyperParameterTrainingJobDefinition.md) object that describes the training jobs that this tuning job launches, including static hyperparameters, input data configuration, output data configuration, resource configuration, and stopping condition\.  
Type: [HyperParameterTrainingJobDefinition](API_HyperParameterTrainingJobDefinition.md) object  
Required: Yes

## Response Syntax<a name="API_CreateHyperParameterTuningJob_ResponseSyntax"></a>

```
{
   "[HyperParameterTuningJobArn](#SageMaker-CreateHyperParameterTuningJob-response-HyperParameterTuningJobArn)": "string"
}
```

## Response Elements<a name="API_CreateHyperParameterTuningJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [HyperParameterTuningJobArn](#API_CreateHyperParameterTuningJob_ResponseSyntax) **   <a name="SageMaker-CreateHyperParameterTuningJob-response-HyperParameterTuningJobArn"></a>
The Amazon Resource Name \(ARN\) of the tuning job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:hyper-parameter-tuning-job/.*` 

## Errors<a name="API_CreateHyperParameterTuningJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceInUse**   
Resource being accessed is in use\.  
HTTP Status Code: 400

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateHyperParameterTuningJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateHyperParameterTuningJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateHyperParameterTuningJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateHyperParameterTuningJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateHyperParameterTuningJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateHyperParameterTuningJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateHyperParameterTuningJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateHyperParameterTuningJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateHyperParameterTuningJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateHyperParameterTuningJob) 