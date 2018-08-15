# TransformInput<a name="API_TransformInput"></a>

Describes the input source of a transform job and the way the transform job consumes it\.

## Contents<a name="API_TransformInput_Contents"></a>

 **CompressionType**   <a name="SageMaker-Type-TransformInput-CompressionType"></a>
Compressing data helps save on storage space\. If your transform data is compressed, specify the compression type\.and Amazon SageMaker will automatically decompress the data for the transform job accordingly\. The default value is `None`\.  
Type: String  
Valid Values:` None | Gzip`   
Required: No

 **ContentType**   <a name="SageMaker-Type-TransformInput-ContentType"></a>
The multipurpose internet mail extension \(MIME\) type of the data\. Amazon SageMaker uses the MIME type with each http call to transfer data to the transform job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

 **DataSource**   <a name="SageMaker-Type-TransformInput-DataSource"></a>
Describes the location of the channel data, meaning the S3 location of the input data that the model can consume\.  
Type: [TransformDataSource](API_TransformDataSource.md) object  
Required: Yes

 **SplitType**   <a name="SageMaker-Type-TransformInput-SplitType"></a>
The method to use to split the transform job's data into smaller batches\. The default value is `None`\. If you don't want to split the data, specify `None`\. If you want to split records on a newline character boundary, specify `Line`\. To split records according to the RecordIO format, specify `RecordIO`\.  
Amazon SageMaker will send maximum number of records per batch in each request up to the MaxPayloadInMB limit\. For more information, see [RecordIO data format](http://mxnet.io/architecture/note_data_loading.html#data-format)\.  
For information about the `RecordIO` format, see [Data Format](http://mxnet.io/architecture/note_data_loading.html#data-format)\.
Type: String  
Valid Values:` None | Line | RecordIO`   
Required: No

## See Also<a name="API_TransformInput_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TransformInput) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TransformInput) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TransformInput) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TransformInput) 