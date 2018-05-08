# OutputDataConfig<a name="API_OutputDataConfig"></a>

Provides information about how to store model training results \(model artifacts\)\.

## Contents<a name="API_OutputDataConfig_Contents"></a>

 **KmsKeyId**   <a name="SageMaker-Type-OutputDataConfig-KmsKeyId"></a>
The AWS Key Management Service \(AWS KMS\) key that Amazon SageMaker uses to encrypt the model artifacts at rest using Amazon S3 server\-side encryption\.   
If the configuration of the output S3 bucket requires server\-side encryption for objects, and you don't provide the KMS key ID, Amazon SageMaker uses the default service key\. For more information, see [KMS\-Managed Encryption Keys](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html) in Amazon Simple Storage Service developer guide\.
 The KMS key policy must grant permission to the IAM role you specify in your `CreateTrainingJob` request\. [Using Key Policies in AWS KMS](http://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the AWS Key Management Service Developer Guide\. 
Type: String  
Length Constraints: Maximum length of 2048\.  
Required: No

 **S3OutputPath**   <a name="SageMaker-Type-OutputDataConfig-S3OutputPath"></a>
Identifies the S3 path where you want Amazon SageMaker to store the model artifacts\. For example, `s3://bucket-name/key-name-prefix`\.   
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_OutputDataConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/OutputDataConfig) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/OutputDataConfig) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/OutputDataConfig) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/OutputDataConfig) 