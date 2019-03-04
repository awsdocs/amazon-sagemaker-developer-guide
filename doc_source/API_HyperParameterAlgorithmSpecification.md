# HyperParameterAlgorithmSpecification<a name="API_HyperParameterAlgorithmSpecification"></a>

Specifies which training algorithm to use for training jobs that a hyperparameter tuning job launches and the metrics to monitor\.

## Contents<a name="API_HyperParameterAlgorithmSpecification_Contents"></a>

 **AlgorithmName**   <a name="SageMaker-Type-HyperParameterAlgorithmSpecification-AlgorithmName"></a>
The name of the resource algorithm to use for the hyperparameter tuning job\. If you specify a value for this parameter, do not specify a value for `TrainingImage`\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 170\.  
Pattern: `(arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:[a-z\-]*\/)?([a-zA-Z0-9]([a-zA-Z0-9-]){0,62})(?<!-)$`   
Required: No

 **MetricDefinitions**   <a name="SageMaker-Type-HyperParameterAlgorithmSpecification-MetricDefinitions"></a>
An array of [MetricDefinition](API_MetricDefinition.md) objects that specify the metrics that the algorithm emits\.  
Type: Array of [MetricDefinition](API_MetricDefinition.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 20 items\.  
Required: No

 **TrainingImage**   <a name="SageMaker-Type-HyperParameterAlgorithmSpecification-TrainingImage"></a>
 The registry path of the Docker image that contains the training algorithm\. For information about Docker registry paths for built\-in algorithms, see [Algorithms Provided by Amazon SageMaker: Common Parameters](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html)\. Amazon SageMaker supports both `registry/repository[:tag]` and `registry/repository[@digest]` image path formats\. For more information, see [Using Your Own Algorithms with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html)\.  
Type: String  
Length Constraints: Maximum length of 255\.  
Required: No

 **TrainingInputMode**   <a name="SageMaker-Type-HyperParameterAlgorithmSpecification-TrainingInputMode"></a>
The input mode that the algorithm supports: File or Pipe\. In File input mode, Amazon SageMaker downloads the training data from Amazon S3 to the storage volume that is attached to the training instance and mounts the directory to the Docker volume for the training container\. In Pipe input mode, Amazon SageMaker streams data directly from Amazon S3 to the container\.   
If you specify File mode, make sure that you provision the storage volume that is attached to the training instance with enough capacity to accommodate the training data downloaded from Amazon S3, the model artifacts, and intermediate information\.  
For more information about input modes, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.   
Type: String  
Valid Values:` Pipe | File`   
Required: Yes

## See Also<a name="API_HyperParameterAlgorithmSpecification_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterAlgorithmSpecification) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterAlgorithmSpecification) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterAlgorithmSpecification) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterAlgorithmSpecification) 