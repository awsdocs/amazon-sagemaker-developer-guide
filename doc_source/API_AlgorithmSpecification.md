# AlgorithmSpecification<a name="API_AlgorithmSpecification"></a>

Specifies the training algorithm to use in a [CreateTrainingJob](http://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html) request\. 

For more information about algorithms provided by Amazon SageMaker, see [Algorithms](http://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\. For information about using your own algorithms, see [Using Your Own Algorithms with Amazon SageMaker](your-algorithms.md)\. 

## Contents<a name="API_AlgorithmSpecification_Contents"></a>

 **TrainingImage**   <a name="SageMaker-Type-AlgorithmSpecification-TrainingImage"></a>
The registry path of the Docker image that contains the training algorithm\. For information about docker registry paths for built\-in algorithms, see [Algorithms Provided by Amazon SageMaker: Common Parameters ](sagemaker-algo-docker-registry-paths.md)\.  
Type: String  
Length Constraints: Maximum length of 255\.  
Required: Yes

 **TrainingInputMode**   <a name="SageMaker-Type-AlgorithmSpecification-TrainingInputMode"></a>
The input mode that the algorithm supports\. For the input modes that Amazon SageMaker algorithms support, see [Algorithms](http://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\. If an algorithm supports the `File` input mode, Amazon SageMaker downloads the training data from S3 to the provisioned ML storage Volume, and mounts the directory to docker volume for training container\. If an algorithm supports the `Pipe` input mode, Amazon SageMaker streams data directly from S3 to the container\.   
 In File mode, make sure you provision ML storage volume with sufficient capacity to accommodate the data download from S3\. In addition to the training data, the ML storage volume also stores the output model\. The algorithm container use ML storage volume to also store intermediate information, if any\.   
 For distributed algorithms using File mode, training data is distributed uniformly, and your training duration is predictable if the input data objects size is approximately same\. Amazon SageMaker does not split the files any further for model training\. If the object sizes are skewed, training won't be optimal as the data distribution is also skewed where one host in a training cluster is overloaded, thus becoming bottleneck in training\.   
Type: String  
Valid Values:` Pipe | File`   
Required: Yes

## See Also<a name="API_AlgorithmSpecification_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/AlgorithmSpecification) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/AlgorithmSpecification) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/AlgorithmSpecification) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/AlgorithmSpecification) 