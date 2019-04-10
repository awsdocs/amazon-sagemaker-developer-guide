# CreateModelPackage<a name="API_CreateModelPackage"></a>

Creates a model package that you can use to create Amazon SageMaker models or list on AWS Marketplace\. Buyers can subscribe to model packages listed on AWS Marketplace to create models in Amazon SageMaker\.

To create a model package by specifying a Docker container that contains your inference code and the Amazon S3 location of your model artifacts, provide values for `InferenceSpecification`\. To create a model from an algorithm resource that you created or subscribed to in AWS Marketplace, provide a value for `SourceAlgorithmSpecification`\.

## Request Syntax<a name="API_CreateModelPackage_RequestSyntax"></a>

```
{
   "[CertifyForMarketplace](#SageMaker-CreateModelPackage-request-CertifyForMarketplace)": boolean,
   "[InferenceSpecification](#SageMaker-CreateModelPackage-request-InferenceSpecification)": { 
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
   "[ModelPackageDescription](#SageMaker-CreateModelPackage-request-ModelPackageDescription)": "string",
   "[ModelPackageName](#SageMaker-CreateModelPackage-request-ModelPackageName)": "string",
   "[SourceAlgorithmSpecification](#SageMaker-CreateModelPackage-request-SourceAlgorithmSpecification)": { 
      "[SourceAlgorithms](API_SourceAlgorithmSpecification.md#SageMaker-Type-SourceAlgorithmSpecification-SourceAlgorithms)": [ 
         { 
            "[AlgorithmName](API_SourceAlgorithm.md#SageMaker-Type-SourceAlgorithm-AlgorithmName)": "string",
            "[ModelDataUrl](API_SourceAlgorithm.md#SageMaker-Type-SourceAlgorithm-ModelDataUrl)": "string"
         }
      ]
   },
   "[ValidationSpecification](#SageMaker-CreateModelPackage-request-ValidationSpecification)": { 
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

## Request Parameters<a name="API_CreateModelPackage_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CertifyForMarketplace](#API_CreateModelPackage_RequestSyntax) **   <a name="SageMaker-CreateModelPackage-request-CertifyForMarketplace"></a>
Whether to certify the model package for listing on AWS Marketplace\.  
Type: Boolean  
Required: No

 ** [InferenceSpecification](#API_CreateModelPackage_RequestSyntax) **   <a name="SageMaker-CreateModelPackage-request-InferenceSpecification"></a>
Specifies details about inference jobs that can be run with models based on this model package, including the following:  
+ The Amazon ECR paths of containers that contain the inference code and model artifacts\.
+ The instance types that the model package supports for transform jobs and real\-time endpoints used for inference\.
+ The input and output content formats that the model package supports for inference\.
Type: [InferenceSpecification](API_InferenceSpecification.md) object  
Required: No

 ** [ModelPackageDescription](#API_CreateModelPackage_RequestSyntax) **   <a name="SageMaker-CreateModelPackage-request-ModelPackageDescription"></a>
A description of the model package\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `[\p{L}\p{M}\p{Z}\p{S}\p{N}\p{P}]*`   
Required: No

 ** [ModelPackageName](#API_CreateModelPackage_RequestSyntax) **   <a name="SageMaker-CreateModelPackage-request-ModelPackageName"></a>
The name of the model package\. The name must have 1 to 63 characters\. Valid characters are a\-z, A\-Z, 0\-9, and \- \(hyphen\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 ** [SourceAlgorithmSpecification](#API_CreateModelPackage_RequestSyntax) **   <a name="SageMaker-CreateModelPackage-request-SourceAlgorithmSpecification"></a>
Details about the algorithm that was used to create the model package\.  
Type: [SourceAlgorithmSpecification](API_SourceAlgorithmSpecification.md) object  
Required: No

 ** [ValidationSpecification](#API_CreateModelPackage_RequestSyntax) **   <a name="SageMaker-CreateModelPackage-request-ValidationSpecification"></a>
Specifies configurations for one or more transform jobs that Amazon SageMaker runs to test the model package\.  
Type: [ModelPackageValidationSpecification](API_ModelPackageValidationSpecification.md) object  
Required: No

## Response Syntax<a name="API_CreateModelPackage_ResponseSyntax"></a>

```
{
   "[ModelPackageArn](#SageMaker-CreateModelPackage-response-ModelPackageArn)": "string"
}
```

## Response Elements<a name="API_CreateModelPackage_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ModelPackageArn](#API_CreateModelPackage_ResponseSyntax) **   <a name="SageMaker-CreateModelPackage-response-ModelPackageArn"></a>
The Amazon Resource Name \(ARN\) of the new model package\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:model-package/.*` 

## Errors<a name="API_CreateModelPackage_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_CreateModelPackage_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateModelPackage) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateModelPackage) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateModelPackage) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateModelPackage) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CreateModelPackage) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateModelPackage) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateModelPackage) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateModelPackage) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateModelPackage) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateModelPackage) 