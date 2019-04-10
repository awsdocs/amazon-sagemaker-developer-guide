# DescribeTrainingJobResponse<a name="API_DescribeTrainingJobResponse"></a>

## Contents<a name="API_DescribeTrainingJobResponse_Contents"></a>

 **AlgorithmSpecification**   <a name="SageMaker-Type-DescribeTrainingJobResponse-AlgorithmSpecification"></a>
Information about the algorithm used for training, and algorithm metadata\.   
Type: [AlgorithmSpecification](API_AlgorithmSpecification.md) object  
Required: Yes

 **CreationTime**   <a name="SageMaker-Type-DescribeTrainingJobResponse-CreationTime"></a>
A timestamp that indicates when the training job was created\.  
Type: Timestamp  
Required: Yes

 **EnableInterContainerTrafficEncryption**   <a name="SageMaker-Type-DescribeTrainingJobResponse-EnableInterContainerTrafficEncryption"></a>
To encrypt all communications between ML compute instances in distributed training, choose `True`\. Encryption provides greater security for distributed training, but training might take longer\. How long it takes depends on the amount of communication between compute instances, especially if you use a deep learning algorithm in distributed training\.  
Type: Boolean  
Required: No

 **EnableNetworkIsolation**   <a name="SageMaker-Type-DescribeTrainingJobResponse-EnableNetworkIsolation"></a>
If you want to allow inbound or outbound network calls, except for calls between peers within a training cluster for distributed training, choose `True`\. If you enable network isolation for training jobs that are configured to use a VPC, Amazon SageMaker downloads and uploads customer data and model artifacts through the specified VPC, but the training container does not have network access\.  
The Semantic Segmentation built\-in algorithm does not support network isolation\.
Type: Boolean  
Required: No

 **FailureReason**   <a name="SageMaker-Type-DescribeTrainingJobResponse-FailureReason"></a>
If the training job failed, the reason it failed\.   
Type: String  
Length Constraints: Maximum length of 1024\.  
Required: No

 **FinalMetricDataList**   <a name="SageMaker-Type-DescribeTrainingJobResponse-FinalMetricDataList"></a>
A collection of `MetricData` objects that specify the names, values, and dates and times that the training algorithm emitted to Amazon CloudWatch\.  
Type: Array of [MetricData](API_MetricData.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 20 items\.  
Required: No

 **HyperParameters**   <a name="SageMaker-Type-DescribeTrainingJobResponse-HyperParameters"></a>
Algorithm\-specific parameters\.   
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Key Pattern: `.*`   
Value Length Constraints: Maximum length of 256\.  
Value Pattern: `.*`   
Required: No

 **InputDataConfig**   <a name="SageMaker-Type-DescribeTrainingJobResponse-InputDataConfig"></a>
An array of `Channel` objects that describes each data input channel\.   
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 8 items\.  
Required: No

 **LabelingJobArn**   <a name="SageMaker-Type-DescribeTrainingJobResponse-LabelingJobArn"></a>
The Amazon Resource Name \(ARN\) of the Amazon SageMaker Ground Truth labeling job that created the transform or training job\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:labeling-job/.*`   
Required: No

 **LastModifiedTime**   <a name="SageMaker-Type-DescribeTrainingJobResponse-LastModifiedTime"></a>
A timestamp that indicates when the status of the training job was last modified\.  
Type: Timestamp  
Required: No

 **ModelArtifacts**   <a name="SageMaker-Type-DescribeTrainingJobResponse-ModelArtifacts"></a>
Information about the Amazon S3 location that is configured for storing model artifacts\.   
Type: [ModelArtifacts](API_ModelArtifacts.md) object  
Required: Yes

 **OutputDataConfig**   <a name="SageMaker-Type-DescribeTrainingJobResponse-OutputDataConfig"></a>
The S3 path where model artifacts that you configured when creating the job are stored\. Amazon SageMaker creates subfolders for model artifacts\.   
Type: [OutputDataConfig](API_OutputDataConfig.md) object  
Required: No

 **ResourceConfig**   <a name="SageMaker-Type-DescribeTrainingJobResponse-ResourceConfig"></a>
Resources, including ML compute instances and ML storage volumes, that are configured for model training\.   
Type: [ResourceConfig](API_ResourceConfig.md) object  
Required: Yes

 **RoleArn**   <a name="SageMaker-Type-DescribeTrainingJobResponse-RoleArn"></a>
The AWS Identity and Access Management \(IAM\) role configured for the training job\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: No

 **SecondaryStatus**   <a name="SageMaker-Type-DescribeTrainingJobResponse-SecondaryStatus"></a>
 Provides detailed information about the state of the training job\. For detailed information on the secondary status of the training job, see `StatusMessage` under [SecondaryStatusTransition](API_SecondaryStatusTransition.md)\.  
Amazon SageMaker provides primary statuses and secondary statuses that apply to each of them:    
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
Valid values for `SecondaryStatus` are subject to change\. 
We no longer support the following secondary statuses:  
+  `LaunchingMLInstances` 
+  `PreparingTrainingStack` 
+  `DownloadingTrainingImage` 
Type: String  
Valid Values:` Starting | LaunchingMLInstances | PreparingTrainingStack | Downloading | DownloadingTrainingImage | Training | Uploading | Stopping | Stopped | MaxRuntimeExceeded | Completed | Failed`   
Required: Yes

 **SecondaryStatusTransitions**   <a name="SageMaker-Type-DescribeTrainingJobResponse-SecondaryStatusTransitions"></a>
A history of all of the secondary statuses that the training job has transitioned through\.  
Type: Array of [SecondaryStatusTransition](API_SecondaryStatusTransition.md) objects  
Required: No

 **StoppingCondition**   <a name="SageMaker-Type-DescribeTrainingJobResponse-StoppingCondition"></a>
The condition under which to stop the training job\.   
Type: [StoppingCondition](API_StoppingCondition.md) object  
Required: Yes

 **TrainingEndTime**   <a name="SageMaker-Type-DescribeTrainingJobResponse-TrainingEndTime"></a>
Indicates the time when the training job ends on training instances\. You are billed for the time interval between the value of `TrainingStartTime` and this time\. For successful jobs and stopped jobs, this is the time after model artifacts are uploaded\. For failed jobs, this is the time when Amazon SageMaker detects a job failure\.  
Type: Timestamp  
Required: No

 **TrainingJobArn**   <a name="SageMaker-Type-DescribeTrainingJobResponse-TrainingJobArn"></a>
The Amazon Resource Name \(ARN\) of the training job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:training-job/.*`   
Required: Yes

 **TrainingJobName**   <a name="SageMaker-Type-DescribeTrainingJobResponse-TrainingJobName"></a>
 Name of the model training job\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **TrainingJobStatus**   <a name="SageMaker-Type-DescribeTrainingJobResponse-TrainingJobStatus"></a>
The status of the training job\.  
Amazon SageMaker provides the following training job statuses:  
+  `InProgress` \- The training is in progress\.
+  `Completed` \- The training job has completed\.
+  `Failed` \- The training job has failed\. To see the reason for the failure, see the `FailureReason` field in the response to a `DescribeTrainingJobResponse` call\.
+  `Stopping` \- The training job is stopping\.
+  `Stopped` \- The training job has stopped\.
For more detailed information, see `SecondaryStatus`\.   
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: Yes

 **TrainingStartTime**   <a name="SageMaker-Type-DescribeTrainingJobResponse-TrainingStartTime"></a>
Indicates the time when the training job starts on training instances\. You are billed for the time interval between this time and the value of `TrainingEndTime`\. The start time in CloudWatch Logs might be later than this time\. The difference is due to the time it takes to download the training data and to the size of the training container\.  
Type: Timestamp  
Required: No

 **TuningJobArn**   <a name="SageMaker-Type-DescribeTrainingJobResponse-TuningJobArn"></a>
The Amazon Resource Name \(ARN\) of the associated hyperparameter tuning job if the training job was launched by a hyperparameter tuning job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:hyper-parameter-tuning-job/.*`   
Required: No

 **VpcConfig**   <a name="SageMaker-Type-DescribeTrainingJobResponse-VpcConfig"></a>
A [VpcConfig](API_VpcConfig.md) object that specifies the VPC that this training job has access to\. For more information, see [Protect Training Jobs by Using an Amazon Virtual Private Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/train-vpc.html)\.  
Type: [VpcConfig](API_VpcConfig.md) object  
Required: No

## See Also<a name="API_DescribeTrainingJobResponse_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeTrainingJobResponse) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeTrainingJobResponse) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DescribeTrainingJobResponse) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeTrainingJobResponse) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeTrainingJobResponse) 