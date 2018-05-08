# ModelArtifacts<a name="API_ModelArtifacts"></a>

Provides information about the location that is configured for storing model artifacts\. 

## Contents<a name="API_ModelArtifacts_Contents"></a>

 **S3ModelArtifacts**   <a name="SageMaker-Type-ModelArtifacts-S3ModelArtifacts"></a>
The path of the S3 object that contains the model artifacts\. For example, `s3://bucket-name/keynameprefix/model.tar.gz`\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_ModelArtifacts_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ModelArtifacts) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ModelArtifacts) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ModelArtifacts) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ModelArtifacts) 