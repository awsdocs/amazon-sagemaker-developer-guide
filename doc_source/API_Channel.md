# Channel<a name="API_Channel"></a>

A channel is a named input source that training algorithms can consume\. 

## Contents<a name="API_Channel_Contents"></a>

 **ChannelName**   <a name="SageMaker-Type-Channel-ChannelName"></a>
The name of the channel\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[A-Za-z0-9\.\-_]+`   
Required: Yes

 **CompressionType**   <a name="SageMaker-Type-Channel-CompressionType"></a>
If training data is compressed, the compression type\. The default value is `None`\. `CompressionType` is used only in Pipe input mode\. In File mode, leave this field unset or set it to None\.  
Type: String  
Valid Values:` None | Gzip`   
Required: No

 **ContentType**   <a name="SageMaker-Type-Channel-ContentType"></a>
The MIME type of the data\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

 **DataSource**   <a name="SageMaker-Type-Channel-DataSource"></a>
The location of the channel data\.  
Type: [DataSource](API_DataSource.md) object  
Required: Yes

 **RecordWrapperType**   <a name="SageMaker-Type-Channel-RecordWrapperType"></a>
  
Specify RecordIO as the value when input data is in raw format but the training algorithm requires the RecordIO format, in which case, Amazon SageMaker wraps each individual S3 object in a RecordIO record\. If the input data is already in RecordIO format, you don't need to set this attribute\. For more information, see [Create a Dataset Using RecordIO](https://mxnet.incubator.apache.org/how_to/recordio.html?highlight=im2rec)\.   
In FILE mode, leave this field unset or set it to None\.  
  
Type: String  
Valid Values:` None | RecordIO`   
Required: No

## See Also<a name="API_Channel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/Channel) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/Channel) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/Channel) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/Channel) 