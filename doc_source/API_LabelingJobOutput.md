# LabelingJobOutput<a name="API_LabelingJobOutput"></a>

Specifies the location of the output produced by the labeling job\. 

## Contents<a name="API_LabelingJobOutput_Contents"></a>

 **FinalActiveLearningModelArn**   <a name="SageMaker-Type-LabelingJobOutput-FinalActiveLearningModelArn"></a>
The Amazon Resource Name \(ARN\) for the most recent Amazon SageMaker model trained as part of automated data labeling\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Required: No

 **OutputDatasetS3Uri**   <a name="SageMaker-Type-LabelingJobOutput-OutputDatasetS3Uri"></a>
The Amazon S3 bucket location of the manifest file for labeled data\.   
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_LabelingJobOutput_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/LabelingJobOutput) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/LabelingJobOutput) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/LabelingJobOutput) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/LabelingJobOutput) 