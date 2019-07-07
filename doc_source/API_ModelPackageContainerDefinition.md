# ModelPackageContainerDefinition<a name="API_ModelPackageContainerDefinition"></a>

Describes the Docker container for the model package\.

## Contents<a name="API_ModelPackageContainerDefinition_Contents"></a>

 **ContainerHostname**   <a name="SageMaker-Type-ModelPackageContainerDefinition-ContainerHostname"></a>
The DNS host name for the Docker container\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 **Image**   <a name="SageMaker-Type-ModelPackageContainerDefinition-Image"></a>
The Amazon EC2 Container Registry \(Amazon ECR\) path where inference code is stored\.  
If you are using your own custom algorithm instead of an algorithm provided by Amazon SageMaker, the inference code must meet Amazon SageMaker requirements\. Amazon SageMaker supports both `registry/repository[:tag]` and `registry/repository[@digest]` image path formats\. For more information, see [Using Your Own Algorithms with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html)\.  
Type: String  
Length Constraints: Maximum length of 255\.  
Pattern: `[\S]+`   
Required: Yes

 **ImageDigest**   <a name="SageMaker-Type-ModelPackageContainerDefinition-ImageDigest"></a>
An MD5 hash of the training algorithm that identifies the Docker image used for training\.  
Type: String  
Length Constraints: Maximum length of 72\.  
Pattern: `^[Ss][Hh][Aa]256:[0-9a-fA-F]{64}$`   
Required: No

 **ModelDataUrl**   <a name="SageMaker-Type-ModelPackageContainerDefinition-ModelDataUrl"></a>
The Amazon S3 path where the model artifacts, which result from model training, are stored\. This path must point to a single `gzip` compressed tar archive \(`.tar.gz` suffix\)\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: No

 **ProductId**   <a name="SageMaker-Type-ModelPackageContainerDefinition-ProductId"></a>
The AWS Marketplace product ID of the model package\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: No

## See Also<a name="API_ModelPackageContainerDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ModelPackageContainerDefinition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ModelPackageContainerDefinition) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ModelPackageContainerDefinition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ModelPackageContainerDefinition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ModelPackageContainerDefinition) 