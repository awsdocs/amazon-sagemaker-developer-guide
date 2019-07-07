# LabelingJobOutputConfig<a name="API_LabelingJobOutputConfig"></a>

Output configuration information for a labeling job\.

## Contents<a name="API_LabelingJobOutputConfig_Contents"></a>

 **KmsKeyId**   <a name="SageMaker-Type-LabelingJobOutputConfig-KmsKeyId"></a>
The AWS Key Management Service ID of the key used to encrypt the output data, if any\.  
If you use a KMS key ID or an alias of your master key, the Amazon SageMaker execution role must include permissions to call `kms:Encrypt`\. If you don't provide a KMS key ID, Amazon SageMaker uses the default KMS key for Amazon S3 for your role's account\. Amazon SageMaker uses server\-side encryption with KMS\-managed keys for `LabelingJobOutputConfig`\. If you use a bucket policy with an `s3:PutObject` permission that only allows objects with server\-side encryption, set the condition key of `s3:x-amz-server-side-encryption` to `"aws:kms"`\. For more information, see [KMS\-Managed Encryption Keys](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html) in the *Amazon Simple Storage Service Developer Guide\.*   
The KMS key policy must grant permission to the IAM role that you specify in your `CreateLabelingJob` request\. For more information, see [Using Key Policies in AWS KMS](http://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `.*`   
Required: No

 **S3OutputPath**   <a name="SageMaker-Type-LabelingJobOutputConfig-S3OutputPath"></a>
The Amazon S3 location to write output data\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_LabelingJobOutputConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/LabelingJobOutputConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/LabelingJobOutputConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/LabelingJobOutputConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/LabelingJobOutputConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/LabelingJobOutputConfig) 