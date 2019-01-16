# ShuffleConfig<a name="API_ShuffleConfig"></a>

A configuration for a shuffle option for input data in a channel\. If you use `S3Prefix` for `S3DataType`, the results of the S3 key prefix matches are shuffled\. If you use `ManifestFile`, the order of the S3 object references in the `ManifestFile` is shuffled\. If you use `AugmentedManifestFile`, the order of the JSON lines in the `AugmentedManifestFile` is shuffled\. The shuffling order is determined using the `Seed` value\.

For Pipe input mode, shuffling is done at the start of every epoch\. With large datasets, this ensures that the order of the training data is different for each epoch, and it helps reduce bias and possible overfitting\. In a multi\-node training job when `ShuffleConfig` is combined with `S3DataDistributionType` of `ShardedByS3Key`, the data is shuffled across nodes so that the content sent to a particular node on the first epoch might be sent to a different node on the second epoch\.

## Contents<a name="API_ShuffleConfig_Contents"></a>

 **Seed**   <a name="SageMaker-Type-ShuffleConfig-Seed"></a>
Determines the shuffling order in `ShuffleConfig` value\.  
Type: Long  
Required: Yes

## See Also<a name="API_ShuffleConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ShuffleConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ShuffleConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ShuffleConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ShuffleConfig) 