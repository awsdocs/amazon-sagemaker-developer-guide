# SecondaryStatusTransition<a name="API_SecondaryStatusTransition"></a>

An array element of [DescribeTrainingJob:SecondaryStatusTransitions](API_DescribeTrainingJob.md#SageMaker-DescribeTrainingJob-response-SecondaryStatusTransitions)\. It provides additional details about a status that the training job has transitioned through\. A training job can be in one of several states, for example, starting, downloading, training, or uploading\. Within each state, there are a number of intermediate states\. For example, within the starting state, Amazon SageMaker could be starting the training job or launching the ML instances\. These transitional states are referred to as the job's secondary status\. 

## Contents<a name="API_SecondaryStatusTransition_Contents"></a>

 **EndTime**   <a name="SageMaker-Type-SecondaryStatusTransition-EndTime"></a>
A timestamp that shows when the training job transitioned out of this secondary status state into another secondary status state or when the training job has ended\.  
Type: Timestamp  
Required: No

 **StartTime**   <a name="SageMaker-Type-SecondaryStatusTransition-StartTime"></a>
A timestamp that shows when the training job transitioned to the current secondary status state\.  
Type: Timestamp  
Required: Yes

 **Status**   <a name="SageMaker-Type-SecondaryStatusTransition-Status"></a>
Contains a secondary status information from a training job\.  
Status might be one of the following secondary statuses:    
InProgress  
+  `Starting` \- Starting the training job\.
+  `Downloading` \- An optional stage for algorithms that support `File` training input mode\. It indicates that data is being downloaded to the ML storage volumes\.
+  `Training` \- Training is in progress\.
+  `Uploading` \- Training is complete and the model artifacts are being uploaded to the S3 location\.  
Completed  
+  `Completed` \- The training job has completed\.  
Failed  
+  `Failed` \- The training job has failed\. The reason for the failure is returned in the `FailureReason` field of `DescribeTrainingJobResponse`\.  
Stopped  
+  `MaxRuntimeExceeded` \- The job stopped because it exceeded the maximum allowed runtime\.
+  `Stopped` \- The training job has stopped\.  
Stopping  
+  `Stopping` \- Stopping the training job\.
We no longer support the following secondary statuses:  
+  `LaunchingMLInstances` 
+  `PreparingTrainingStack` 
+  `DownloadingTrainingImage` 
Type: String  
Valid Values:` Starting | LaunchingMLInstances | PreparingTrainingStack | Downloading | DownloadingTrainingImage | Training | Uploading | Stopping | Stopped | MaxRuntimeExceeded | Completed | Failed`   
Required: Yes

 **StatusMessage**   <a name="SageMaker-Type-SecondaryStatusTransition-StatusMessage"></a>
A detailed description of the progress within a secondary status\.   
Amazon SageMaker provides secondary statuses and status messages that apply to each of them:    
Starting  
+ Starting the training job\.
+ Launching requested ML instances\.
+ Insufficient capacity error from EC2 while launching instances, retrying\!
+ Launched instance was unhealthy, replacing it\!
+ Preparing the instances for training\.  
Training  
+ Downloading the training image\.
+ Training image download completed\. Training in progress\.
Status messages are subject to change\. Therefore, we recommend not including them in code that programmatically initiates actions\. For examples, don't use status messages in if statements\.
To have an overview of your training job's progress, view `TrainingJobStatus` and `SecondaryStatus` in [DescribeTrainingJobResponse](API_DescribeTrainingJobResponse.md), and `StatusMessage` together\. For example, at the start of a training job, you might see the following:  
+  `TrainingJobStatus` \- InProgress
+  `SecondaryStatus` \- Training
+  `StatusMessage` \- Downloading the training image
Type: String  
Required: No

## See Also<a name="API_SecondaryStatusTransition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/SecondaryStatusTransition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/SecondaryStatusTransition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/SecondaryStatusTransition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/SecondaryStatusTransition) 