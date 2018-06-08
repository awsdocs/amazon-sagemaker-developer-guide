# HyperParameterTuningJobSummary<a name="API_HyperParameterTuningJobSummary"></a>

Provides summary information about a hyperparameter tuning job\.

## Contents<a name="API_HyperParameterTuningJobSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-CreationTime"></a>
The date and time that the tuning job was created\.  
Type: Timestamp  
Required: Yes

 **HyperParameterTuningEndTime**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningEndTime"></a>
The date and time that the tuning job ended\.  
Type: Timestamp  
Required: No

 **HyperParameterTuningJobArn**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningJobArn"></a>
The Amazon Resource Name \(ARN\) of the tuning job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:hyper-parameter-tuning-job/.*`   
Required: Yes

 **HyperParameterTuningJobName**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningJobName"></a>
The name of the tuning job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **HyperParameterTuningJobStatus**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-HyperParameterTuningJobStatus"></a>
The status of the tuning job\.  
Type: String  
Valid Values:` Completed | InProgress | Failed | Stopped | Stopping`   
Required: Yes

 **LastModifiedTime**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-LastModifiedTime"></a>
The date and time that the tuning job was modified\.  
Type: Timestamp  
Required: No

 **ObjectiveStatusCounters**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-ObjectiveStatusCounters"></a>
The [ObjectiveStatusCounters](API_ObjectiveStatusCounters.md) object that specifies the numbers of training jobs, categorized by objective metric status, that this tuning job launched\.  
Type: [ObjectiveStatusCounters](API_ObjectiveStatusCounters.md) object  
Required: Yes

 **ResourceLimits**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-ResourceLimits"></a>
The [ResourceLimits](API_ResourceLimits.md) object that specifies the maximum number of training jobs and parallel training jobs allowed for this tuning job\.  
Type: [ResourceLimits](API_ResourceLimits.md) object  
Required: No

 **Strategy**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-Strategy"></a>
Specifies the search strategy hyperparameter tuning uses to choose which hyperparameters to use for each iteration\. Currently, the only valid value is Bayesian\.  
Type: String  
Valid Values:` Bayesian`   
Required: Yes

 **TrainingJobStatusCounters**   <a name="SageMaker-Type-HyperParameterTuningJobSummary-TrainingJobStatusCounters"></a>
The [TrainingJobStatusCounters](API_TrainingJobStatusCounters.md) object that specifies the numbers of training jobs, categorized by status, that this tuning job launched\.  
Type: [TrainingJobStatusCounters](API_TrainingJobStatusCounters.md) object  
Required: Yes

## See Also<a name="API_HyperParameterTuningJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterTuningJobSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterTuningJobSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterTuningJobSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterTuningJobSummary) 