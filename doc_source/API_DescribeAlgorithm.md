# DescribeAlgorithm<a name="API_DescribeAlgorithm"></a>

Returns a description of the specified algorithm that is in your account\.

## Request Syntax<a name="API_DescribeAlgorithm_RequestSyntax"></a>

```
{
   "[AlgorithmName](#SageMaker-DescribeAlgorithm-request-AlgorithmName)": "string"
}
```

## Request Parameters<a name="API_DescribeAlgorithm_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [AlgorithmName](#API_DescribeAlgorithm_RequestSyntax) **   <a name="SageMaker-DescribeAlgorithm-request-AlgorithmName"></a>
The name of the algorithm to describe\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 170\.  
Pattern: `(arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:[a-z\-]*\/)?([a-zA-Z0-9]([a-zA-Z0-9-]){0,62})(?<!-)$`   
Required: Yes

## Response Syntax<a name="API_DescribeAlgorithm_ResponseSyntax"></a>

```
{
   "[AlgorithmArn](#SageMaker-DescribeAlgorithm-response-AlgorithmArn)": "string",
   "[AlgorithmDescription](#SageMaker-DescribeAlgorithm-response-AlgorithmDescription)": "string",
   "[AlgorithmName](#SageMaker-DescribeAlgorithm-response-AlgorithmName)": "string",
   "[AlgorithmStatus](#SageMaker-DescribeAlgorithm-response-AlgorithmStatus)": "string",
   "[AlgorithmStatusDetails](#SageMaker-DescribeAlgorithm-response-AlgorithmStatusDetails)": { 
      "[ImageScanStatuses](API_AlgorithmStatusDetails.md#SageMaker-Type-AlgorithmStatusDetails-ImageScanStatuses)": [ 
         { 
            "[FailureReason](API_AlgorithmStatusItem.md#SageMaker-Type-AlgorithmStatusItem-FailureReason)": "string",
            "[Name](API_AlgorithmStatusItem.md#SageMaker-Type-AlgorithmStatusItem-Name)": "string",
            "[Status](API_AlgorithmStatusItem.md#SageMaker-Type-AlgorithmStatusItem-Status)": "string"
         }
      ],
      "[ValidationStatuses](API_AlgorithmStatusDetails.md#SageMaker-Type-AlgorithmStatusDetails-ValidationStatuses)": [ 
         { 
            "[FailureReason](API_AlgorithmStatusItem.md#SageMaker-Type-AlgorithmStatusItem-FailureReason)": "string",
            "[Name](API_AlgorithmStatusItem.md#SageMaker-Type-AlgorithmStatusItem-Name)": "string",
            "[Status](API_AlgorithmStatusItem.md#SageMaker-Type-AlgorithmStatusItem-Status)": "string"
         }
      ]
   },
   "[CertifyForMarketplace](#SageMaker-DescribeAlgorithm-response-CertifyForMarketplace)": boolean,
   "[CreationTime](#SageMaker-DescribeAlgorithm-response-CreationTime)": number,
   "[InferenceSpecification](#SageMaker-DescribeAlgorithm-response-InferenceSpecification)": { 
      "[Containers](API_InferenceSpecification.md#SageMaker-Type-InferenceSpecification-Containers)": [ 
         { 
            "[ContainerHostname](API_ModelPackageContainerDefinition.md#SageMaker-Type-ModelPackageContainerDefinition-ContainerHostname)": "string",
            "[Image](API_ModelPackageContainerDefinition.md#SageMaker-Type-ModelPackageContainerDefinition-Image)": "string",
            "[ImageDigest](API_ModelPackageContainerDefinition.md#SageMaker-Type-ModelPackageContainerDefinition-ImageDigest)": "string",
            "[ModelDataUrl](API_ModelPackageContainerDefinition.md#SageMaker-Type-ModelPackageContainerDefinition-ModelDataUrl)": "string",
            "[ProductId](API_ModelPackageContainerDefinition.md#SageMaker-Type-ModelPackageContainerDefinition-ProductId)": "string"
         }
      ],
      "[SupportedContentTypes](API_InferenceSpecification.md#SageMaker-Type-InferenceSpecification-SupportedContentTypes)": [ "string" ],
      "[SupportedRealtimeInferenceInstanceTypes](API_InferenceSpecification.md#SageMaker-Type-InferenceSpecification-SupportedRealtimeInferenceInstanceTypes)": [ "string" ],
      "[SupportedResponseMIMETypes](API_InferenceSpecification.md#SageMaker-Type-InferenceSpecification-SupportedResponseMIMETypes)": [ "string" ],
      "[SupportedTransformInstanceTypes](API_InferenceSpecification.md#SageMaker-Type-InferenceSpecification-SupportedTransformInstanceTypes)": [ "string" ]
   },
   "[ProductId](#SageMaker-DescribeAlgorithm-response-ProductId)": "string",
   "[TrainingSpecification](#SageMaker-DescribeAlgorithm-response-TrainingSpecification)": { 
      "[MetricDefinitions](API_TrainingSpecification.md#SageMaker-Type-TrainingSpecification-MetricDefinitions)": [ 
         { 
            "[Name](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Name)": "string",
            "[Regex](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Regex)": "string"
         }
      ],
      "[SupportedHyperParameters](API_TrainingSpecification.md#SageMaker-Type-TrainingSpecification-SupportedHyperParameters)": [ 
         { 
            "[DefaultValue](API_HyperParameterSpecification.md#SageMaker-Type-HyperParameterSpecification-DefaultValue)": "string",
            "[Description](API_HyperParameterSpecification.md#SageMaker-Type-HyperParameterSpecification-Description)": "string",
            "[IsRequired](API_HyperParameterSpecification.md#SageMaker-Type-HyperParameterSpecification-IsRequired)": boolean,
            "[IsTunable](API_HyperParameterSpecification.md#SageMaker-Type-HyperParameterSpecification-IsTunable)": boolean,
            "[Name](API_HyperParameterSpecification.md#SageMaker-Type-HyperParameterSpecification-Name)": "string",
            "[Range](API_HyperParameterSpecification.md#SageMaker-Type-HyperParameterSpecification-Range)": { 
               "[CategoricalParameterRangeSpecification](API_ParameterRange.md#SageMaker-Type-ParameterRange-CategoricalParameterRangeSpecification)": { 
                  "[Values](API_CategoricalParameterRangeSpecification.md#SageMaker-Type-CategoricalParameterRangeSpecification-Values)": [ "string" ]
               },
               "[ContinuousParameterRangeSpecification](API_ParameterRange.md#SageMaker-Type-ParameterRange-ContinuousParameterRangeSpecification)": { 
                  "[MaxValue](API_ContinuousParameterRangeSpecification.md#SageMaker-Type-ContinuousParameterRangeSpecification-MaxValue)": "string",
                  "[MinValue](API_ContinuousParameterRangeSpecification.md#SageMaker-Type-ContinuousParameterRangeSpecification-MinValue)": "string"
               },
               "[IntegerParameterRangeSpecification](API_ParameterRange.md#SageMaker-Type-ParameterRange-IntegerParameterRangeSpecification)": { 
                  "[MaxValue](API_IntegerParameterRangeSpecification.md#SageMaker-Type-IntegerParameterRangeSpecification-MaxValue)": "string",
                  "[MinValue](API_IntegerParameterRangeSpecification.md#SageMaker-Type-IntegerParameterRangeSpecification-MinValue)": "string"
               }
            },
            "[Type](API_HyperParameterSpecification.md#SageMaker-Type-HyperParameterSpecification-Type)": "string"
         }
      ],
      "[SupportedTrainingInstanceTypes](API_TrainingSpecification.md#SageMaker-Type-TrainingSpecification-SupportedTrainingInstanceTypes)": [ "string" ],
      "[SupportedTuningJobObjectiveMetrics](API_TrainingSpecification.md#SageMaker-Type-TrainingSpecification-SupportedTuningJobObjectiveMetrics)": [ 
         { 
            "[MetricName](API_HyperParameterTuningJobObjective.md#SageMaker-Type-HyperParameterTuningJobObjective-MetricName)": "string",
            "[Type](API_HyperParameterTuningJobObjective.md#SageMaker-Type-HyperParameterTuningJobObjective-Type)": "string"
         }
      ],
      "[SupportsDistributedTraining](API_TrainingSpecification.md#SageMaker-Type-TrainingSpecification-SupportsDistributedTraining)": boolean,
      "[TrainingChannels](API_TrainingSpecification.md#SageMaker-Type-TrainingSpecification-TrainingChannels)": [ 
         { 
            "[Description](API_ChannelSpecification.md#SageMaker-Type-ChannelSpecification-Description)": "string",
            "[IsRequired](API_ChannelSpecification.md#SageMaker-Type-ChannelSpecification-IsRequired)": boolean,
            "[Name](API_ChannelSpecification.md#SageMaker-Type-ChannelSpecification-Name)": "string",
            "[SupportedCompressionTypes](API_ChannelSpecification.md#SageMaker-Type-ChannelSpecification-SupportedCompressionTypes)": [ "string" ],
            "[SupportedContentTypes](API_ChannelSpecification.md#SageMaker-Type-ChannelSpecification-SupportedContentTypes)": [ "string" ],
            "[SupportedInputModes](API_ChannelSpecification.md#SageMaker-Type-ChannelSpecification-SupportedInputModes)": [ "string" ]
         }
      ],
      "[TrainingImage](API_TrainingSpecification.md#SageMaker-Type-TrainingSpecification-TrainingImage)": "string",
      "[TrainingImageDigest](API_TrainingSpecification.md#SageMaker-Type-TrainingSpecification-TrainingImageDigest)": "string"
   },
   "[ValidationSpecification](#SageMaker-DescribeAlgorithm-response-ValidationSpecification)": { 
      "[ValidationProfiles](API_AlgorithmValidationSpecification.md#SageMaker-Type-AlgorithmValidationSpecification-ValidationProfiles)": [ 
         { 
            "[ProfileName](API_AlgorithmValidationProfile.md#SageMaker-Type-AlgorithmValidationProfile-ProfileName)": "string",
            "[TrainingJobDefinition](API_AlgorithmValidationProfile.md#SageMaker-Type-AlgorithmValidationProfile-TrainingJobDefinition)": { 
               "[HyperParameters](API_TrainingJobDefinition.md#SageMaker-Type-TrainingJobDefinition-HyperParameters)": { 
                  "string" : "string" 
               },
               "[InputDataConfig](API_TrainingJobDefinition.md#SageMaker-Type-TrainingJobDefinition-InputDataConfig)": [ 
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
               "[OutputDataConfig](API_TrainingJobDefinition.md#SageMaker-Type-TrainingJobDefinition-OutputDataConfig)": { 
                  "[KmsKeyId](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-KmsKeyId)": "string",
                  "[S3OutputPath](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-S3OutputPath)": "string"
               },
               "[ResourceConfig](API_TrainingJobDefinition.md#SageMaker-Type-TrainingJobDefinition-ResourceConfig)": { 
                  "[InstanceCount](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceCount)": number,
                  "[InstanceType](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceType)": "string",
                  "[VolumeKmsKeyId](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeKmsKeyId)": "string",
                  "[VolumeSizeInGB](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeSizeInGB)": number
               },
               "[StoppingCondition](API_TrainingJobDefinition.md#SageMaker-Type-TrainingJobDefinition-StoppingCondition)": { 
                  "[MaxRuntimeInSeconds](API_StoppingCondition.md#SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds)": number
               },
               "[TrainingInputMode](API_TrainingJobDefinition.md#SageMaker-Type-TrainingJobDefinition-TrainingInputMode)": "string"
            },
            "[TransformJobDefinition](API_AlgorithmValidationProfile.md#SageMaker-Type-AlgorithmValidationProfile-TransformJobDefinition)": { 
               "[BatchStrategy](API_TransformJobDefinition.md#SageMaker-Type-TransformJobDefinition-BatchStrategy)": "string",
               "[Environment](API_TransformJobDefinition.md#SageMaker-Type-TransformJobDefinition-Environment)": { 
                  "string" : "string" 
               },
               "[MaxConcurrentTransforms](API_TransformJobDefinition.md#SageMaker-Type-TransformJobDefinition-MaxConcurrentTransforms)": number,
               "[MaxPayloadInMB](API_TransformJobDefinition.md#SageMaker-Type-TransformJobDefinition-MaxPayloadInMB)": number,
               "[TransformInput](API_TransformJobDefinition.md#SageMaker-Type-TransformJobDefinition-TransformInput)": { 
                  "[CompressionType](API_TransformInput.md#SageMaker-Type-TransformInput-CompressionType)": "string",
                  "[ContentType](API_TransformInput.md#SageMaker-Type-TransformInput-ContentType)": "string",
                  "[DataSource](API_TransformInput.md#SageMaker-Type-TransformInput-DataSource)": { 
                     "[S3DataSource](API_TransformDataSource.md#SageMaker-Type-TransformDataSource-S3DataSource)": { 
                        "[S3DataType](API_TransformS3DataSource.md#SageMaker-Type-TransformS3DataSource-S3DataType)": "string",
                        "[S3Uri](API_TransformS3DataSource.md#SageMaker-Type-TransformS3DataSource-S3Uri)": "string"
                     }
                  },
                  "[SplitType](API_TransformInput.md#SageMaker-Type-TransformInput-SplitType)": "string"
               },
               "[TransformOutput](API_TransformJobDefinition.md#SageMaker-Type-TransformJobDefinition-TransformOutput)": { 
                  "[Accept](API_TransformOutput.md#SageMaker-Type-TransformOutput-Accept)": "string",
                  "[AssembleWith](API_TransformOutput.md#SageMaker-Type-TransformOutput-AssembleWith)": "string",
                  "[KmsKeyId](API_TransformOutput.md#SageMaker-Type-TransformOutput-KmsKeyId)": "string",
                  "[S3OutputPath](API_TransformOutput.md#SageMaker-Type-TransformOutput-S3OutputPath)": "string"
               },
               "[TransformResources](API_TransformJobDefinition.md#SageMaker-Type-TransformJobDefinition-TransformResources)": { 
                  "[InstanceCount](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceCount)": number,
                  "[InstanceType](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceType)": "string",
                  "[VolumeKmsKeyId](API_TransformResources.md#SageMaker-Type-TransformResources-VolumeKmsKeyId)": "string"
               }
            }
         }
      ],
      "[ValidationRole](API_AlgorithmValidationSpecification.md#SageMaker-Type-AlgorithmValidationSpecification-ValidationRole)": "string"
   }
}
```

## Response Elements<a name="API_DescribeAlgorithm_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AlgorithmArn](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-AlgorithmArn"></a>
The Amazon Resource Name \(ARN\) of the algorithm\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:algorithm/.*` 

 ** [AlgorithmDescription](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-AlgorithmDescription"></a>
A brief summary about the algorithm\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `[\p{L}\p{M}\p{Z}\p{S}\p{N}\p{P}]*` 

 ** [AlgorithmName](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-AlgorithmName"></a>
The name of the algorithm being described\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$` 

 ** [AlgorithmStatus](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-AlgorithmStatus"></a>
The current status of the algorithm\.  
Type: String  
Valid Values:` Pending | InProgress | Completed | Failed | Deleting` 

 ** [AlgorithmStatusDetails](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-AlgorithmStatusDetails"></a>
Details about the current status of the algorithm\.  
Type: [AlgorithmStatusDetails](API_AlgorithmStatusDetails.md) object

 ** [CertifyForMarketplace](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-CertifyForMarketplace"></a>
Whether the algorithm is certified to be listed in AWS Marketplace\.  
Type: Boolean

 ** [CreationTime](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-CreationTime"></a>
A timestamp specifying when the algorithm was created\.  
Type: Timestamp

 ** [InferenceSpecification](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-InferenceSpecification"></a>
Details about inference jobs that the algorithm runs\.  
Type: [InferenceSpecification](API_InferenceSpecification.md) object

 ** [ProductId](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-ProductId"></a>
The product identifier of the algorithm\.  
Type: String

 ** [TrainingSpecification](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-TrainingSpecification"></a>
Details about training jobs run by this algorithm\.  
Type: [TrainingSpecification](API_TrainingSpecification.md) object

 ** [ValidationSpecification](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="SageMaker-DescribeAlgorithm-response-ValidationSpecification"></a>
Details about configurations for one or more training jobs that Amazon SageMaker runs to test the algorithm\.  
Type: [AlgorithmValidationSpecification](API_AlgorithmValidationSpecification.md) object

## Errors<a name="API_DescribeAlgorithm_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeAlgorithm_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeAlgorithm) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeAlgorithm) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeAlgorithm) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeAlgorithm) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeAlgorithm) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeAlgorithm) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeAlgorithm) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeAlgorithm) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeAlgorithm) 