# TransformOutput<a name="API_TransformOutput"></a>

Describes the results of a transform job\.

## Contents<a name="API_TransformOutput_Contents"></a>

 **Accept**   <a name="SageMaker-Type-TransformOutput-Accept"></a>
The MIME type used to specify the output data\. Amazon SageMaker uses the MIME type with each http call to transfer data from the transform job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `.*`   
Required: No

 **AssembleWith**   <a name="SageMaker-Type-TransformOutput-AssembleWith"></a>
Defines how to assemble the results of the transform job as a single S3 object\. Choose a format that is most convenient to you\. To concatenate the results in binary format, specify `None`\. To add a newline character at the end of every transformed record, specify `Line`\.  
Type: String  
Valid Values:` None | Line`   
Required: No

 **KmsKeyId**   <a name="SageMaker-Type-TransformOutput-KmsKeyId"></a>
The AWS Key Management Service \(AWS KMS\) key that Amazon SageMaker uses to encrypt the model artifacts at rest using Amazon S3 server\-side encryption\. The `KmsKeyId` can be any of the following formats:   
+ // KMS Key ID

   `"1234abcd-12ab-34cd-56ef-1234567890ab"` 
+ // Amazon Resource Name \(ARN\) of a KMS Key

   `"arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"` 
+ // KMS Key Alias

   `"alias/ExampleAlias"` 
+ // Amazon Resource Name \(ARN\) of a KMS Key Alias

   `"arn:aws:kms:us-west-2:111122223333:alias/ExampleAlias"` 
If you don't provide a KMS key ID, Amazon SageMaker uses the default KMS key for Amazon S3 for your role's account\. For more information, see [KMS\-Managed Encryption Keys](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html) in the *Amazon Simple Storage Service Developer Guide\.*   
The KMS key policy must grant permission to the IAM role that you specify in your `CreateTramsformJob` request\. For more information, see [Using Key Policies in AWS KMS](http://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `.*`   
Required: No

 **S3OutputPath**   <a name="SageMaker-Type-TransformOutput-S3OutputPath"></a>
The Amazon S3 path where you want Amazon SageMaker to store the results of the transform job\. For example, `s3://bucket-name/key-name-prefix`\.  
For every S3 object used as input for the transform job, batch transform stores the transformed data with an \.`out` suffix in a corresponding subfolder in the location in the output prefix\. For example, for the input data stored at `s3://bucket-name/input-name-prefix/dataset01/data.csv`, batch transform stores the transformed data at `s3://bucket-name/output-name-prefix/input-name-prefix/data.csv.out`\. Batch transform doesn't upload partially processed objects\. For an input S3 object that contains multiple records, it creates an \.`out` file only if the transform job succeeds on the entire file\. When the input contains multiple S3 objects, the batch transform job processes the listed S3 objects and uploads only the output for successfully processed objects\. If any object fails in the transform job batch transform marks the job as failed to prompt investigation\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_TransformOutput_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TransformOutput) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TransformOutput) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/TransformOutput) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TransformOutput) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TransformOutput) 