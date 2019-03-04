# TransformInput<a name="API_TransformInput"></a>

Describes the input source of a transform job and the way the transform job consumes it\.

## Contents<a name="API_TransformInput_Contents"></a>

 **CompressionType**   <a name="SageMaker-Type-TransformInput-CompressionType"></a>
If your transform data is compressed, specify the compression type\. Amazon SageMaker automatically decompresses the data for the transform job accordingly\. The default value is `None`\.  
Type: String  
Valid Values:` None | Gzip`   
Required: No

 **ContentType**   <a name="SageMaker-Type-TransformInput-ContentType"></a>
The multipurpose internet mail extension \(MIME\) type of the data\. Amazon SageMaker uses the MIME type with each http call to transfer data to the transform job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

 **DataSource**   <a name="SageMaker-Type-TransformInput-DataSource"></a>
Describes the location of the channel data, which is, the S3 location of the input data that the model can consume\.  
Type: [TransformDataSource](API_TransformDataSource.md) object  
Required: Yes

 **SplitType**   <a name="SageMaker-Type-TransformInput-SplitType"></a>
The method to use to split the transform job's data files into smaller batches\. Splitting is necessary when the total size of each object is too large to fit in a single request\. You can also use data splitting to improve performance by processing multiple concurrent mini\-batches\. The default value for `SplitType` is `None`, which indicates that input data files are not split, and request payloads contain the entire contents of an input object\. Set the value of this parameter to `Line` to split records on a newline character boundary\. `SplitType` also supports a number of record\-oriented binary data formats\.  
When splitting is enabled, the size of a mini\-batch depends on the values of the `BatchStrategy` and `MaxPayloadInMB` parameters\. When the value of `BatchStrategy` is `MultiRecord`, Amazon SageMaker sends the maximum number of records in each request, up to the `MaxPayloadInMB` limit\. If the value of `BatchStrategy` is `SingleRecord`, Amazon SageMaker sends individual records in each request\.  
Some data formats represent a record as a binary payload wrapped with extra padding bytes\. When splitting is applied to a binary data format, padding is removed if the value of `BatchStrategy` is set to `SingleRecord`\. Padding is not removed if the value of `BatchStrategy` is set to `MultiRecord`\.  
For more information about the RecordIO, see [Data Format](http://mxnet.io/architecture/note_data_loading.html#data-format) in the MXNet documentation\. For more information about the TFRecord, see [Consuming TFRecord data](https://www.tensorflow.org/guide/datasets#consuming_tfrecord_data) in the TensorFlow documentation\.
Type: String  
Valid Values:` None | Line | RecordIO | TFRecord`   
Required: No

## See Also<a name="API_TransformInput_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TransformInput) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TransformInput) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TransformInput) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TransformInput) 