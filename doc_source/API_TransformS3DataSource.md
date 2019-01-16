# TransformS3DataSource<a name="API_TransformS3DataSource"></a>

Describes the S3 data source\.

## Contents<a name="API_TransformS3DataSource_Contents"></a>

 **S3DataType**   <a name="SageMaker-Type-TransformS3DataSource-S3DataType"></a>
If you choose `S3Prefix`, `S3Uri` identifies a key name prefix\. Amazon SageMaker uses all objects with the specified key name prefix for batch transform\.   
If you choose `ManifestFile`, `S3Uri` identifies an object that is a manifest file containing a list of object keys that you want Amazon SageMaker to use for batch transform\.   
Type: String  
Valid Values:` ManifestFile | S3Prefix | AugmentedManifestFile`   
Required: Yes

 **S3Uri**   <a name="SageMaker-Type-TransformS3DataSource-S3Uri"></a>
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

   The preceding JSON matches the following `S3Uris`: 

   `s3://customer_bucket/some/prefix/relative/path/to/custdata-1` 

   `s3://customer_bucket/some/prefix/relative/path/custdata-1` 

   `...` 

   The complete set of `S3Uris` in this manifest constitutes the input data for the channel for this datasource\. The object that each `S3Uris` points to must be readable by the IAM role that Amazon SageMaker uses to perform tasks on your behalf\.
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_TransformS3DataSource_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TransformS3DataSource) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TransformS3DataSource) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TransformS3DataSource) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TransformS3DataSource) 