# TrainingJobSummary<a name="API_TrainingJobSummary"></a>

Provides summary information about a training job\.

## Contents<a name="API_TrainingJobSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-TrainingJobSummary-CreationTime"></a>
A timestamp that shows when the training job was created\.  
Type: Timestamp  
Required: Yes

 **LastModifiedTime**   <a name="SageMaker-Type-TrainingJobSummary-LastModifiedTime"></a>
 Timestamp when the training job was last modified\.   
Type: Timestamp  
Required: No

 **TrainingEndTime**   <a name="SageMaker-Type-TrainingJobSummary-TrainingEndTime"></a>
A timestamp that shows when the training job ended\. This field is set only if the training job has one of the terminal statuses \(`Completed`, `Failed`, or `Stopped`\)\.   
Type: Timestamp  
Required: No

 **TrainingJobArn**   <a name="SageMaker-Type-TrainingJobSummary-TrainingJobArn"></a>
The Amazon Resource Name \(ARN\) of the training job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:training-job/.*`   
Required: Yes

 **TrainingJobName**   <a name="SageMaker-Type-TrainingJobSummary-TrainingJobName"></a>
The name of the training job that you want a summary for\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **TrainingJobStatus**   <a name="SageMaker-Type-TrainingJobSummary-TrainingJobStatus"></a>
The status of the training job\.  
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: Yes

## See Also<a name="API_TrainingJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TrainingJobSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TrainingJobSummary) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/TrainingJobSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TrainingJobSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TrainingJobSummary) 