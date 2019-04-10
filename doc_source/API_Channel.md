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
Pattern: `.*`   
Required: No

 **DataSource**   <a name="SageMaker-Type-Channel-DataSource"></a>
The location of the channel data\.  
Type: [DataSource](API_DataSource.md) object  
Required: Yes

 **InputMode**   <a name="SageMaker-Type-Channel-InputMode"></a>
\(Optional\) The input mode to use for the data channel in a training job\. If you don't set a value for `InputMode`, Amazon SageMaker uses the value set for `TrainingInputMode`\. Use this parameter to override the `TrainingInputMode` setting in a [AlgorithmSpecification](API_AlgorithmSpecification.md) request when you have a channel that needs a different input mode from the training job's general setting\. To download the data from Amazon Simple Storage Service \(Amazon S3\) to the provisioned ML storage volume, and mount the directory to a Docker volume, use `File` input mode\. To stream data directly from Amazon S3 to the container, choose `Pipe` input mode\.  
To use a model for incremental training, choose `File` input model\.  
Type: String  
Valid Values:` Pipe | File`   
Required: No

 **RecordWrapperType**   <a name="SageMaker-Type-Channel-RecordWrapperType"></a>
Specify RecordIO as the value when input data is in raw format but the training algorithm requires the RecordIO format\. In this case, Amazon SageMaker wraps each individual S3 object in a RecordIO record\. If the input data is already in RecordIO format, you don't need to set this attribute\. For more information, see [Create a Dataset Using RecordIO](https://mxnet.incubator.apache.org/architecture/note_data_loading.html#data-format)\.   
In File mode, leave this field unset or set it to None\.  
Type: String  
Valid Values:` None | RecordIO`   
Required: No

 **ShuffleConfig**   <a name="SageMaker-Type-Channel-ShuffleConfig"></a>
A configuration for a shuffle option for input data in a channel\. If you use `S3Prefix` for `S3DataType`, this shuffles the results of the S3 key prefix matches\. If you use `ManifestFile`, the order of the S3 object references in the `ManifestFile` is shuffled\. If you use `AugmentedManifestFile`, the order of the JSON lines in the `AugmentedManifestFile` is shuffled\. The shuffling order is determined using the `Seed` value\.  
For Pipe input mode, shuffling is done at the start of every epoch\. With large datasets this ensures that the order of the training data is different for each epoch, it helps reduce bias and possible overfitting\. In a multi\-node training job when ShuffleConfig is combined with `S3DataDistributionType` of `ShardedByS3Key`, the data is shuffled across nodes so that the content sent to a particular node on the first epoch might be sent to a different node on the second epoch\.  
Type: [ShuffleConfig](API_ShuffleConfig.md) object  
Required: No

## See Also<a name="API_Channel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/Channel) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/Channel) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/Channel) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/Channel) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/Channel) 