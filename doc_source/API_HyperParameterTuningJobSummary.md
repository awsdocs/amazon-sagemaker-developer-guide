# HyperParameterTuningJobSummary<a name="API_HyperParameterTuningJobSummary"></a>

## Contents<a name="API_HyperParameterTuningJobSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-CreationTime"></a>
Type: Timestamp  
Required: Yes

 **HyperParameterTuningEndTime**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningEndTime"></a>
Type: Timestamp  
Required: No

 **HyperParameterTuningJobArn**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningJobArn"></a>
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

 **HyperParameterTuningJobName**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningJobName"></a>
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **HyperParameterTuningJobStatus**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningJobStatus"></a>
Type: String  
Valid Values:` Completed | InProgress | Failed | Stopped | Stopping`   
Required: Yes

 **LastModifiedTime**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-LastModifiedTime"></a>
Type: Timestamp  
Required: No

 **ResourceLimits**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-ResourceLimits"></a>
Type: [ResourceLimits](API_ResourceLimits.md) object  
Required: No

 **Strategy**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-Strategy"></a>
Type: String  
Valid Values:` Bayesian`   
Required: Yes

 **TrainingJobCounters**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-TrainingJobCounters"></a>
Type: [TrainingJobCounters](API_TrainingJobCounters.md) object  
Required: No

## See Also<a name="API_HyperParameterTuningJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterTuningJobSummary) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterTuningJobSummary) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterTuningJobSummary) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterTuningJobSummary) 