# LabelingJobResourceConfig<a name="API_LabelingJobResourceConfig"></a>

Provides configuration information for labeling jobs\.

## Contents<a name="API_LabelingJobResourceConfig_Contents"></a>

 **VolumeKmsKeyId**   <a name="SageMaker-Type-LabelingJobResourceConfig-VolumeKmsKeyId"></a>
The AWS Key Management Service \(AWS KMS\) key that Amazon SageMaker uses to encrypt data on the storage volume attached to the ML compute instance\(s\) that run the training job\. The `VolumeKmsKeyId` can be any of the following formats:  
+ // KMS Key ID

   `"1234abcd-12ab-34cd-56ef-1234567890ab"` 
+ // Amazon Resource Name \(ARN\) of a KMS Key

   `"arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"` 
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `.*`   
Required: No

## See Also<a name="API_LabelingJobResourceConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/LabelingJobResourceConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/LabelingJobResourceConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/LabelingJobResourceConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/LabelingJobResourceConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/LabelingJobResourceConfig) 