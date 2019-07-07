# ContainerDefinition<a name="API_ContainerDefinition"></a>

Describes the container, as part of model definition\.

## Contents<a name="API_ContainerDefinition_Contents"></a>

 **ContainerHostname**   <a name="SageMaker-Type-ContainerDefinition-ContainerHostname"></a>
This parameter is ignored for models that contain only a `PrimaryContainer`\.  
When a `ContainerDefinition` is part of an inference pipeline, the value of ths parameter uniquely identifies the container for the purposes of logging and metrics\. For information, see [Use Logs and Metrics to Monitor an Inference Pipeline](http://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipeline-logs-metrics.html)\. If you don't specify a value for this parameter for a `ContainerDefinition` that is part of an inference pipeline, a unique name is automatically assigned based on the position of the `ContainerDefinition` in the pipeline\. If you specify a value for the `ContainerHostName` for any `ContainerDefinition` that is part of an inference pipeline, you must specify a value for the `ContainerHostName` parameter of every `ContainerDefinition` in that pipeline\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 **Environment**   <a name="SageMaker-Type-ContainerDefinition-Environment"></a>
The environment variables to set in the Docker container\. Each key and value in the `Environment` string to string map can have length of up to 1024\. We support up to 16 entries in the map\.   
Type: String to string map  
Key Length Constraints: Maximum length of 1024\.  
Key Pattern: `[a-zA-Z_][a-zA-Z0-9_]*`   
Value Length Constraints: Maximum length of 1024\.  
Value Pattern: `[\S\s]*`   
Required: No

 **Image**   <a name="SageMaker-Type-ContainerDefinition-Image"></a>
The Amazon EC2 Container Registry \(Amazon ECR\) path where inference code is stored\. If you are using your own custom algorithm instead of an algorithm provided by Amazon SageMaker, the inference code must meet Amazon SageMaker requirements\. Amazon SageMaker supports both `registry/repository[:tag]` and `registry/repository[@digest]` image path formats\. For more information, see [Using Your Own Algorithms with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html)   
Type: String  
Length Constraints: Maximum length of 255\.  
Pattern: `[\S]+`   
Required: No

 **ModelDataUrl**   <a name="SageMaker-Type-ContainerDefinition-ModelDataUrl"></a>
The S3 path where the model artifacts, which result from model training, are stored\. This path must point to a single gzip compressed tar archive \(\.tar\.gz suffix\)\. The S3 path is required for Amazon SageMaker built\-in algorithms, but not if you use your own algorithms\. For more information on built\-in algorithms, see [Common Parameters](http://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html)\.   
If you provide a value for this parameter, Amazon SageMaker uses AWS Security Token Service to download model artifacts from the S3 path you provide\. AWS STS is activated in your IAM user account by default\. If you previously deactivated AWS STS for a region, you need to reactivate AWS STS for that region\. For more information, see [Activating and Deactivating AWS STS in an AWS Region](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *AWS Identity and Access Management User Guide*\.  
If you use a built\-in algorithm to create a model, Amazon SageMaker requires that you provide a S3 path to the model artifacts in `ModelDataUrl`\.
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: No

 **ModelPackageName**   <a name="SageMaker-Type-ContainerDefinition-ModelPackageName"></a>
The name or Amazon Resource Name \(ARN\) of the model package to use to create the model\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 170\.  
Pattern: `(arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:[a-z\-]*\/)?([a-zA-Z0-9]([a-zA-Z0-9-]){0,62})(?<!-)$`   
Required: No

## See Also<a name="API_ContainerDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ContainerDefinition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ContainerDefinition) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ContainerDefinition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ContainerDefinition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ContainerDefinition) 