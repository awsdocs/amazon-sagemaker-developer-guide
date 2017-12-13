# ContainerDefinition<a name="API_ContainerDefinition"></a>

Describes the container, as part of model definition\.

## Contents<a name="API_ContainerDefinition_Contents"></a>

 **ContainerHostname**   
The DNS host name for the container after Amazon SageMaker deploys it\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 **Environment**   
The environment variables to set in the Docker container\. Each key and value in the `Environment` string to string map can have length of up to 1024\. We support up to 16 entries in the map\.   
Type: String to string map  
Key Length Constraints: Maximum length of 1024\.  
Key Pattern: `[a-zA-Z_][a-zA-Z0-9_]*`   
Value Length Constraints: Maximum length of 1024\.  
Required: No

 **Image**   
The Amazon EC2 Container Registry \(Amazon ECR\) path where inference code is stored\. If you are using your own custom algorithm instead of an algorithm provided by Amazon SageMaker, the inference code must meet Amazon SageMaker requirements\. For more information, see [Using Your Own Algorithms with Amazon SageMaker](http://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html)   
Type: String  
Length Constraints: Maximum length of 255\.  
Pattern: `[\S]+`   
Required: Yes

 **ModelDataUrl**   
The S3 path where the model artifacts, which result from model training, are stored\. This path must point to a single gzip compressed tar archive \(\.tar\.gz suffix\)\.   
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: No

## See Also<a name="API_ContainerDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ContainerDefinition) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ContainerDefinition) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ContainerDefinition) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ContainerDefinition) 