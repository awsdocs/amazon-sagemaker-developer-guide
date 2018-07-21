# TransformOutput<a name="API_TransformOutput"></a>

## Contents<a name="API_TransformOutput_Contents"></a>

 **AssembleWith**   <a name="SageMaker-Type-TransformOutput-AssembleWith"></a>
Type: String  
Valid Values:` None | Line | RecordIO`   
Required: No

 **CompressionType**   <a name="SageMaker-Type-TransformOutput-CompressionType"></a>
Type: String  
Valid Values:` None | Gzip`   
Required: No

 **KmsKeyId**   <a name="SageMaker-Type-TransformOutput-KmsKeyId"></a>
Type: String  
Length Constraints: Maximum length of 2048\.  
Required: No

 **S3OutputPath**   <a name="SageMaker-Type-TransformOutput-S3OutputPath"></a>
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_TransformOutput_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TransformOutput) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TransformOutput) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TransformOutput) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TransformOutput) 