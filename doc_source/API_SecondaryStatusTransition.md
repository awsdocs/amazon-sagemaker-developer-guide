# SecondaryStatusTransition<a name="API_SecondaryStatusTransition"></a>

Specifies a secondary status the job has transitioned into\. It includes a start timestamp and later an end timestamp\. The end timestamp is added either after the job transitions to a different secondary status or after the job has ended\.

## Contents<a name="API_SecondaryStatusTransition_Contents"></a>

 **EndTime**   <a name="SageMaker-Type-SecondaryStatusTransition-EndTime"></a>
A timestamp that shows when the secondary status has ended and the job has transitioned into another secondary status\. The `EndTime` timestamp is also set after the training job has ended\.  
Type: Timestamp  
Required: No

 **StartTime**   <a name="SageMaker-Type-SecondaryStatusTransition-StartTime"></a>
A timestamp that shows when the training job has entered this secondary status\.  
Type: Timestamp  
Required: Yes

 **Status**   <a name="SageMaker-Type-SecondaryStatusTransition-Status"></a>
Provides granular information about the system state\. For more information, see `SecondaryStatus` under the [DescribeTrainingJob](API_DescribeTrainingJob.md) response elements\.  
Type: String  
Valid Values:` Starting | LaunchingMLInstances | PreparingTrainingStack | Downloading | DownloadingTrainingImage | Training | Uploading | Stopping | Stopped | MaxRuntimeExceeded | Completed | Failed`   
Required: Yes

 **StatusMessage**   <a name="SageMaker-Type-SecondaryStatusTransition-StatusMessage"></a>
Shows a brief description and other information about the secondary status\. For example, the `LaunchingMLInstances` secondary status could show a status message of "Insufficent capacity error while launching instances"\.  
Type: String  
Required: No

## See Also<a name="API_SecondaryStatusTransition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/SecondaryStatusTransition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/SecondaryStatusTransition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/SecondaryStatusTransition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/SecondaryStatusTransition) 