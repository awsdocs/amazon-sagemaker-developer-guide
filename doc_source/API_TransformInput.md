# TransformInput<a name="API_TransformInput"></a>

## Contents<a name="API_TransformInput_Contents"></a>

 **CompressionType**   <a name="SageMaker-Type-TransformInput-CompressionType"></a>
Type: String  
Valid Values:` None | Gzip`   
Required: No

 **ContentType**   <a name="SageMaker-Type-TransformInput-ContentType"></a>
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

 **DataSource**   <a name="SageMaker-Type-TransformInput-DataSource"></a>
Describes the location of the channel data\.  
Type: [DataSource](API_DataSource.md) object  
Required: Yes

 **SplitType**   <a name="SageMaker-Type-TransformInput-SplitType"></a>
Type: String  
Valid Values:` None | Line | RecordIO`   
Required: No

## See Also<a name="API_TransformInput_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TransformInput) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TransformInput) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TransformInput) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TransformInput) 