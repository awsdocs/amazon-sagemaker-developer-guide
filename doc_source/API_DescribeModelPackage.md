# DescribeModelPackage<a name="API_DescribeModelPackage"></a>

Returns a description of the specified model package, which is used to create Amazon SageMaker models or list them on AWS Marketplace\.

To create models in Amazon SageMaker, buyers can subscribe to model packages listed on AWS Marketplace\.

## Request Syntax<a name="API_DescribeModelPackage_RequestSyntax"></a>

```
{
   "[ModelPackageName](#SageMaker-DescribeModelPackage-request-ModelPackageName)": "string"
}
```

## Request Parameters<a name="API_DescribeModelPackage_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [ModelPackageName](#API_DescribeModelPackage_RequestSyntax) **   <a name="SageMaker-DescribeModelPackage-request-ModelPackageName"></a>
The name of the model package to describe\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 170\.  
Pattern: `(arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:[a-z\-]*\/)?([a-zA-Z0-9]([a-zA-Z0-9-]){0,62})(?<!-)$`   
Required: Yes

## Response Syntax<a name="API_DescribeModelPackage_ResponseSyntax"></a>

```
{
   "[CertifyForMarketplace](#SageMaker-DescribeModelPackage-response-CertifyForMarketplace)": boolean,
   "[CreationTime](#SageMaker-DescribeModelPackage-response-CreationTime)": number,
   "[InferenceSpecification](#SageMaker-DescribeModelPackage-response-InferenceSpecification)": { 
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
   "[ModelPackageArn](#SageMaker-DescribeModelPackage-response-ModelPackageArn)": "string",
   "[ModelPackageDescription](#SageMaker-DescribeModelPackage-response-ModelPackageDescription)": "string",
   "[ModelPackageName](#SageMaker-DescribeModelPackage-response-ModelPackageName)": "string",
   "[ModelPackageStatus](#SageMaker-DescribeModelPackage-response-ModelPackageStatus)": "string",
   "[ModelPackageStatusDetails](#SageMaker-DescribeModelPackage-response-ModelPackageStatusDetails)": { 
      "[ImageScanStatuses](API_ModelPackageStatusDetails.md#SageMaker-Type-ModelPackageStatusDetails-ImageScanStatuses)": [ 
         { 
            "[FailureReason](API_ModelPackageStatusItem.md#SageMaker-Type-ModelPackageStatusItem-FailureReason)": "string",
            "[Name](API_ModelPackageStatusItem.md#SageMaker-Type-ModelPackageStatusItem-Name)": "string",
            "[Status](API_ModelPackageStatusItem.md#SageMaker-Type-ModelPackageStatusItem-Status)": "string"
         }
      ],
      "[ValidationStatuses](API_ModelPackageStatusDetails.md#SageMaker-Type-ModelPackageStatusDetails-ValidationStatuses)": [ 
         { 
            "[FailureReason](API_ModelPackageStatusItem.md#SageMaker-Type-ModelPackageStatusItem-FailureReason)": "string",
            "[Name](API_ModelPackageStatusItem.md#SageMaker-Type-ModelPackageStatusItem-Name)": "string",
            "[Status](API_ModelPackageStatusItem.md#SageMaker-Type-ModelPackageStatusItem-Status)": "string"
         }
      ]
   },
   "[SourceAlgorithmSpecification](#SageMaker-DescribeModelPackage-response-SourceAlgorithmSpecification)": { 
      "[SourceAlgorithms](API_SourceAlgorithmSpecification.md#SageMaker-Type-SourceAlgorithmSpecification-SourceAlgorithms)": [ 
         { 
            "[AlgorithmName](API_SourceAlgorithm.md#SageMaker-Type-SourceAlgorithm-AlgorithmName)": "string",
            "[ModelDataUrl](API_SourceAlgorithm.md#SageMaker-Type-SourceAlgorithm-ModelDataUrl)": "string"
         }
      ]
   },
   "[ValidationSpecification](#SageMaker-DescribeModelPackage-response-ValidationSpecification)": { 
      "[ValidationProfiles](API_ModelPackageValidationSpecification.md#SageMaker-Type-ModelPackageValidationSpecification-ValidationProfiles)": [ 
         { 
            "[ProfileName](API_ModelPackageValidationProfile.md#SageMaker-Type-ModelPackageValidationProfile-ProfileName)": "string",
            "[TransformJobDefinition](API_ModelPackageValidationProfile.md#SageMaker-Type-ModelPackageValidationProfile-TransformJobDefinition)": { 
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
      "[ValidationRole](API_ModelPackageValidationSpecification.md#SageMaker-Type-ModelPackageValidationSpecification-ValidationRole)": "string"
   }
}
```

## Response Elements<a name="API_DescribeModelPackage_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CertifyForMarketplace](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-CertifyForMarketplace"></a>
Whether the model package is certified for listing on AWS Marketplace\.  
Type: Boolean

 ** [CreationTime](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-CreationTime"></a>
A timestamp specifying when the model package was created\.  
Type: Timestamp

 ** [InferenceSpecification](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-InferenceSpecification"></a>
Details about inference jobs that can be run with models based on this model package\.  
Type: [InferenceSpecification](API_InferenceSpecification.md) object

 ** [ModelPackageArn](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-ModelPackageArn"></a>
The Amazon Resource Name \(ARN\) of the model package\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:model-package/.*` 

 ** [ModelPackageDescription](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-ModelPackageDescription"></a>
A brief summary of the model package\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `[\p{L}\p{M}\p{Z}\p{S}\p{N}\p{P}]*` 

 ** [ModelPackageName](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-ModelPackageName"></a>
The name of the model package being described\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$` 

 ** [ModelPackageStatus](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-ModelPackageStatus"></a>
The current status of the model package\.  
Type: String  
Valid Values:` Pending | InProgress | Completed | Failed | Deleting` 

 ** [ModelPackageStatusDetails](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-ModelPackageStatusDetails"></a>
Details about the current status of the model package\.  
Type: [ModelPackageStatusDetails](API_ModelPackageStatusDetails.md) object

 ** [SourceAlgorithmSpecification](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-SourceAlgorithmSpecification"></a>
Details about the algorithm that was used to create the model package\.  
Type: [SourceAlgorithmSpecification](API_SourceAlgorithmSpecification.md) object

 ** [ValidationSpecification](#API_DescribeModelPackage_ResponseSyntax) **   <a name="SageMaker-DescribeModelPackage-response-ValidationSpecification"></a>
Configurations for one or more transform jobs that Amazon SageMaker runs to test the model package\.  
Type: [ModelPackageValidationSpecification](API_ModelPackageValidationSpecification.md) object

## Errors<a name="API_DescribeModelPackage_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeModelPackage_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeModelPackage) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeModelPackage) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeModelPackage) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeModelPackage) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeModelPackage) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeModelPackage) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeModelPackage) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeModelPackage) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeModelPackage) 