# CreateAlgorithm<a name="API_CreateAlgorithm"></a>

Create a machine learning algorithm that you can use in Amazon SageMaker and list in the AWS Marketplace\.

## Request Syntax<a name="API_CreateAlgorithm_RequestSyntax"></a>

```
{
   "[AlgorithmDescription](#SageMaker-CreateAlgorithm-request-AlgorithmDescription)": "string",
   "[AlgorithmName](#SageMaker-CreateAlgorithm-request-AlgorithmName)": "string",
   "[CertifyForMarketplace](#SageMaker-CreateAlgorithm-request-CertifyForMarketplace)": boolean,
   "[InferenceSpecification](#SageMaker-CreateAlgorithm-request-InferenceSpecification)": { 
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
   "[TrainingSpecification](#SageMaker-CreateAlgorithm-request-TrainingSpecification)": { 
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
   "[ValidationSpecification](#SageMaker-CreateAlgorithm-request-ValidationSpecification)": { 
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

## Request Parameters<a name="API_CreateAlgorithm_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [AlgorithmDescription](#API_CreateAlgorithm_RequestSyntax) **   <a name="SageMaker-CreateAlgorithm-request-AlgorithmDescription"></a>
A description of the algorithm\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `[\p{L}\p{M}\p{Z}\p{S}\p{N}\p{P}]*`   
Required: No

 ** [AlgorithmName](#API_CreateAlgorithm_RequestSyntax) **   <a name="SageMaker-CreateAlgorithm-request-AlgorithmName"></a>
The name of the algorithm\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 ** [CertifyForMarketplace](#API_CreateAlgorithm_RequestSyntax) **   <a name="SageMaker-CreateAlgorithm-request-CertifyForMarketplace"></a>
Whether to certify the algorithm so that it can be listed in AWS Marketplace\.  
Type: Boolean  
Required: No

 ** [InferenceSpecification](#API_CreateAlgorithm_RequestSyntax) **   <a name="SageMaker-CreateAlgorithm-request-InferenceSpecification"></a>
Specifies details about inference jobs that the algorithm runs, including the following:  
+ The Amazon ECR paths of containers that contain the inference code and model artifacts\.
+ The instance types that the algorithm supports for transform jobs and real\-time endpoints used for inference\.
+ The input and output content formats that the algorithm supports for inference\.
Type: [InferenceSpecification](API_InferenceSpecification.md) object  
Required: No

 ** [TrainingSpecification](#API_CreateAlgorithm_RequestSyntax) **   <a name="SageMaker-CreateAlgorithm-request-TrainingSpecification"></a>
Specifies details about training jobs run by this algorithm, including the following:  
+ The Amazon ECR path of the container and the version digest of the algorithm\.
+ The hyperparameters that the algorithm supports\.
+ The instance types that the algorithm supports for training\.
+ Whether the algorithm supports distributed training\.
+ The metrics that the algorithm emits to Amazon CloudWatch\.
+ Which metrics that the algorithm emits can be used as the objective metric for hyperparameter tuning jobs\.
+ The input channels that the algorithm supports for training data\. For example, an algorithm might support `train`, `validation`, and `test` channels\.
Type: [TrainingSpecification](API_TrainingSpecification.md) object  
Required: Yes

 ** [ValidationSpecification](#API_CreateAlgorithm_RequestSyntax) **   <a name="SageMaker-CreateAlgorithm-request-ValidationSpecification"></a>
Specifies configurations for one or more training jobs and that Amazon SageMaker runs to test the algorithm's training code and, optionally, one or more batch transform jobs that Amazon SageMaker runs to test the algorithm's inference code\.  
Type: [AlgorithmValidationSpecification](API_AlgorithmValidationSpecification.md) object  
Required: No

## Response Syntax<a name="API_CreateAlgorithm_ResponseSyntax"></a>

```
{
   "[AlgorithmArn](#SageMaker-CreateAlgorithm-response-AlgorithmArn)": "string"
}
```

## Response Elements<a name="API_CreateAlgorithm_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AlgorithmArn](#API_CreateAlgorithm_ResponseSyntax) **   <a name="SageMaker-CreateAlgorithm-response-AlgorithmArn"></a>
The Amazon Resource Name \(ARN\) of the new algorithm\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:algorithm/.*` 

## Errors<a name="API_CreateAlgorithm_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_CreateAlgorithm_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateAlgorithm) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateAlgorithm) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateAlgorithm) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateAlgorithm) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateAlgorithm) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateAlgorithm) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateAlgorithm) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateAlgorithm) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateAlgorithm) 