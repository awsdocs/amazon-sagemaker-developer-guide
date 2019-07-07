# LabelingJobAlgorithmsConfig<a name="API_LabelingJobAlgorithmsConfig"></a>

Provides configuration information for auto\-labeling of your data objects\. A `LabelingJobAlgorithmsConfig` object must be supplied in order to use auto\-labeling\.

## Contents<a name="API_LabelingJobAlgorithmsConfig_Contents"></a>

 **InitialActiveLearningModelArn**   <a name="SageMaker-Type-LabelingJobAlgorithmsConfig-InitialActiveLearningModelArn"></a>
At the end of an auto\-label job Amazon SageMaker Ground Truth sends the Amazon Resource Nam \(ARN\) of the final model used for auto\-labeling\. You can use this model as the starting point for subsequent similar jobs by providing the ARN of the model here\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:model/.*`   
Required: No

 **LabelingJobAlgorithmSpecificationArn**   <a name="SageMaker-Type-LabelingJobAlgorithmsConfig-LabelingJobAlgorithmSpecificationArn"></a>
Specifies the Amazon Resource Name \(ARN\) of the algorithm used for auto\-labeling\. You must select one of the following ARNs:  
+  *Image classification* 

   `arn:aws:sagemaker:region:027400017018:labeling-job-algorithm-specification/image-classification` 
+  *Text classification* 

   `arn:aws:sagemaker:region:027400017018:labeling-job-algorithm-specification/text-classification` 
+  *Object detection* 

   `arn:aws:sagemaker:region:027400017018:labeling-job-algorithm-specification/object-detection` 
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `arn:.*`   
Required: Yes

 **LabelingJobResourceConfig**   <a name="SageMaker-Type-LabelingJobAlgorithmsConfig-LabelingJobResourceConfig"></a>
Provides configuration information for a labeling job\.  
Type: [LabelingJobResourceConfig](API_LabelingJobResourceConfig.md) object  
Required: No

## See Also<a name="API_LabelingJobAlgorithmsConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/LabelingJobAlgorithmsConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/LabelingJobAlgorithmsConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/LabelingJobAlgorithmsConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/LabelingJobAlgorithmsConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/LabelingJobAlgorithmsConfig) 