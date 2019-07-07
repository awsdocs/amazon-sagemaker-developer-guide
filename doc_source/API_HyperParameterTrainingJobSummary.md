# HyperParameterTrainingJobSummary<a name="API_HyperParameterTrainingJobSummary"></a>

Specifies summary information about a training job\.

## Contents<a name="API_HyperParameterTrainingJobSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-CreationTime"></a>
The date and time that the training job was created\.  
Type: Timestamp  
Required: Yes

 **FailureReason**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-FailureReason"></a>
The reason that the training job failed\.   
Type: String  
Length Constraints: Maximum length of 1024\.  
Required: No

 **FinalHyperParameterTuningJobObjectiveMetric**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-FinalHyperParameterTuningJobObjectiveMetric"></a>
The [FinalHyperParameterTuningJobObjectiveMetric](API_FinalHyperParameterTuningJobObjectiveMetric.md) object that specifies the value of the objective metric of the tuning job that launched this training job\.  
Type: [FinalHyperParameterTuningJobObjectiveMetric](API_FinalHyperParameterTuningJobObjectiveMetric.md) object  
Required: No

 **ObjectiveStatus**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-ObjectiveStatus"></a>
The status of the objective metric for the training job:  
+ Succeeded: The final objective metric for the training job was evaluated by the hyperparameter tuning job and used in the hyperparameter tuning process\.
+ Pending: The training job is in progress and evaluation of its final objective metric is pending\.
+ Failed: The final objective metric for the training job was not evaluated, and was not used in the hyperparameter tuning process\. This typically occurs when the training job failed or did not emit an objective metric\.
Type: String  
Valid Values:` Succeeded | Pending | Failed`   
Required: No

 **TrainingEndTime**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TrainingEndTime"></a>
Specifies the time when the training job ends on training instances\. You are billed for the time interval between the value of `TrainingStartTime` and this time\. For successful jobs and stopped jobs, this is the time after model artifacts are uploaded\. For failed jobs, this is the time when Amazon SageMaker detects a job failure\.  
Type: Timestamp  
Required: No

 **TrainingJobArn**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobArn"></a>
The Amazon Resource Name \(ARN\) of the training job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:training-job/.*`   
Required: Yes

 **TrainingJobName**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobName"></a>
The name of the training job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **TrainingJobStatus**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TrainingJobStatus"></a>
The status of the training job\.  
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: Yes

 **TrainingStartTime**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TrainingStartTime"></a>
The date and time that the training job started\.  
Type: Timestamp  
Required: No

 **TunedHyperParameters**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TunedHyperParameters"></a>
A list of the hyperparameters for which you specified ranges to search\.  
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Key Pattern: `.*`   
Value Length Constraints: Maximum length of 256\.  
Value Pattern: `.*`   
Required: Yes

 **TuningJobName**   <a name="SageMaker-Type-HyperParameterTrainingJobSummary-TuningJobName"></a>
The HyperParameter tuning job that launched the training job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

## See Also<a name="API_HyperParameterTrainingJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterTrainingJobSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterTrainingJobSummary) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/HyperParameterTrainingJobSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterTrainingJobSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterTrainingJobSummary) 