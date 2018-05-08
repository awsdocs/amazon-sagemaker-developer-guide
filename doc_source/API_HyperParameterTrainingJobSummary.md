# HyperParameterTrainingJobSummary<a name="API_HyperParameterTrainingJobSummary"></a>

## Contents<a name="API_HyperParameterTrainingJobSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-CreationTime"></a>
Type: Timestamp  
Required: Yes

 **FinalHyperParameterTuningJobObjectiveMetric**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-FinalHyperParameterTuningJobObjectiveMetric"></a>
Type: [FinalHyperParameterTuningJobObjectiveMetric](API_FinalHyperParameterTuningJobObjectiveMetric.md) object  
Required: No

 **TrainingEndTime**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TrainingEndTime"></a>
Type: Timestamp  
Required: No

 **TrainingJobArn**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobArn"></a>
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[\p{Alnum}\-]*:[0-9]{12}:training-job/.*`   
Required: Yes

 **TrainingJobName**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobName"></a>
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **TrainingJobStatus**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobStatus"></a>
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: Yes

## See Also<a name="API_HyperParameterTrainingJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterTrainingJobSummary) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterTrainingJobSummary) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterTrainingJobSummary) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterTrainingJobSummary) 