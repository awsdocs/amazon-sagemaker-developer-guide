# S3DataSource<a name="API_S3DataSource"></a>

Describes the S3 data source\.

## Contents<a name="API_S3DataSource_Contents"></a>

 **S3DataDistributionType**   
If you want Amazon SageMaker to replicate the entire dataset on each ML compute instance that is launched for model training, specify `FullyReplicated`\.   
If you want Amazon SageMaker to replicate a subset of data on each ML compute instance that is launched for model training, specify `ShardedByS3Key`\. If there are *n* ML compute instances launched for a training job, each instance gets approximately 1/*n* of the number of S3 objects\. In this case, model training on each machine uses only the subset of training data\.   
Don't choose more ML compute instances for training than available S3 objects\. If you do, some nodes won't get any data and you will pay for nodes that aren't getting any training data\. This applies in both FILE and PIPE modes\. Keep this in mind when developing algorithms\.   
In distributed training, where you use multiple ML compute EC2 instances, you might choose `ShardedByS3Key`\. If the algorithm requires copying training data to the ML storage volume \(when `TrainingInputMode` is set to `File`\), this copies 1/*n* of the number of objects\.   
Type: String  
Valid Values:` FullyReplicated | ShardedByS3Key`   
Required: No

 **S3DataType**   
If you choose `S3Prefix`, `S3Uri` identifies a key name prefix\. Amazon SageMaker uses all objects with the specified key name prefix for model training\.   
If you choose `ManifestFile`, `S3Uri` identifies an object that is a manifest file containing a list of object keys that you want Amazon SageMaker to use for model training\.   
Type: String  
Valid Values:` ManifestFile | S3Prefix`   
Required: Yes

 **S3Uri**   
Depending on the value specified for the `S3DataType`, identifies either a key name prefix or a manifest\. For example:   

+  A key name prefix might look like this: `s3://bucketname/exampleprefix`\. 

+  A manifest might look like this: `s3://bucketname/example.manifest` 

   The manifest is an S3 object which is a JSON file with the following format: 

   `[` 

   ` {"prefix": "s3://customer_bucket/some/prefix/"},` 

   ` "relative/path/to/custdata-1",` 

   ` "relative/path/custdata-2",` 

   ` ...` 

   ` ]` 

   The preceding JSON matches the following `s3Uris`: 

   `s3://customer_bucket/some/prefix/relative/path/to/custdata-1` 

   `s3://customer_bucket/some/prefix/relative/path/custdata-1` 

   `...` 

   The complete set of `s3uris` in this manifest constitutes the input data for the channel for this datasource\. The object that each `s3uris` points to must readable by the IAM role that Amazon SageMaker uses to perform tasks on your behalf\. 
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_S3DataSource_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/S3DataSource) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/S3DataSource) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/S3DataSource) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/S3DataSource) 