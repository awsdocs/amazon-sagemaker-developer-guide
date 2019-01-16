# DescribeTrainingJob<a name="API_DescribeTrainingJob"></a>

Returns information about a training job\.

## Request Syntax<a name="API_DescribeTrainingJob_RequestSyntax"></a>

```
{
   "[TrainingJobName](#SageMaker-DescribeTrainingJob-request-TrainingJobName)": "string"
}
```

## Request Parameters<a name="API_DescribeTrainingJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [TrainingJobName](#API_DescribeTrainingJob_RequestSyntax) **   <a name="SageMaker-DescribeTrainingJob-request-TrainingJobName"></a>
The name of the training job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeTrainingJob_ResponseSyntax"></a>

```
{
   "[AlgorithmSpecification](#SageMaker-DescribeTrainingJob-response-AlgorithmSpecification)": { 
      "[AlgorithmName](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-AlgorithmName)": "string",
      "[MetricDefinitions](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-MetricDefinitions)": [ 
         { 
            "[Name](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Name)": "string",
            "[Regex](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Regex)": "string"
         }
      ],
      "[TrainingImage](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-TrainingImage)": "string",
      "[TrainingInputMode](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-TrainingInputMode)": "string"
   },
   "[CreationTime](#SageMaker-DescribeTrainingJob-response-CreationTime)": number,
   "[EnableInterContainerTrafficEncryption](#SageMaker-DescribeTrainingJob-response-EnableInterContainerTrafficEncryption)": boolean,
   "[EnableNetworkIsolation](#SageMaker-DescribeTrainingJob-response-EnableNetworkIsolation)": boolean,
   "[FailureReason](#SageMaker-DescribeTrainingJob-response-FailureReason)": "string",
   "[FinalMetricDataList](#SageMaker-DescribeTrainingJob-response-FinalMetricDataList)": [ 
      { 
         "[MetricName](API_MetricData.md#SageMaker-Type-MetricData-MetricName)": "string",
         "[Timestamp](API_MetricData.md#SageMaker-Type-MetricData-Timestamp)": number,
         "[Value](API_MetricData.md#SageMaker-Type-MetricData-Value)": number
      }
   ],
   "[HyperParameters](#SageMaker-DescribeTrainingJob-response-HyperParameters)": { 
      "string" : "string" 
   },
   "[InputDataConfig](#SageMaker-DescribeTrainingJob-response-InputDataConfig)": [ 
      { 
         "[ChannelName](API_Channel.md#SageMaker-Type-Channel-ChannelName)": "string",
         "[CompressionType](API_Channel.md#SageMaker-Type-Channel-CompressionType)": "string",
         "[ContentType](API_Channel.md#SageMaker-Type-Channel-ContentType)": "string",
         "[DataSource](API_Channel.md#SageMaker-Type-Channel-DataSource)": { 
            "[S3DataSource](API_DataSource.md#SageMaker-Type-DataSource-S3DataSource)": { 
               "[AttributeNames](API_S3DataSource.md#SageMaker-Type-S3DataSource-AttributeNames)": [ "string" ],
               "[S3DataDistributionType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataDistributionType)": "string",
               "[S3DataType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataType)": "string",
               "[S3Uri](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3Uri)": "string"
            }
         },
         "[InputMode](API_Channel.md#SageMaker-Type-Channel-InputMode)": "string",
         "[RecordWrapperType](API_Channel.md#SageMaker-Type-Channel-RecordWrapperType)": "string",
         "[ShuffleConfig](API_Channel.md#SageMaker-Type-Channel-ShuffleConfig)": { 
            "[Seed](API_ShuffleConfig.md#SageMaker-Type-ShuffleConfig-Seed)": number
         }
      }
   ],
   "[LabelingJobArn](#SageMaker-DescribeTrainingJob-response-LabelingJobArn)": "string",
   "[LastModifiedTime](#SageMaker-DescribeTrainingJob-response-LastModifiedTime)": number,
   "[ModelArtifacts](#SageMaker-DescribeTrainingJob-response-ModelArtifacts)": { 
      "[S3ModelArtifacts](API_ModelArtifacts.md#SageMaker-Type-ModelArtifacts-S3ModelArtifacts)": "string"
   },
   "[OutputDataConfig](#SageMaker-DescribeTrainingJob-response-OutputDataConfig)": { 
      "[KmsKeyId](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-KmsKeyId)": "string",
      "[S3OutputPath](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-S3OutputPath)": "string"
   },
   "[ResourceConfig](#SageMaker-DescribeTrainingJob-response-ResourceConfig)": { 
      "[InstanceCount](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceCount)": number,
      "[InstanceType](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceType)": "string",
      "[VolumeKmsKeyId](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeKmsKeyId)": "string",
      "[VolumeSizeInGB](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeSizeInGB)": number
   },
   "[RoleArn](#SageMaker-DescribeTrainingJob-response-RoleArn)": "string",
   "[SecondaryStatus](#SageMaker-DescribeTrainingJob-response-SecondaryStatus)": "string",
   "[SecondaryStatusTransitions](#SageMaker-DescribeTrainingJob-response-SecondaryStatusTransitions)": [ 
      { 
         "[EndTime](API_SecondaryStatusTransition.md#SageMaker-Type-SecondaryStatusTransition-EndTime)": number,
         "[StartTime](API_SecondaryStatusTransition.md#SageMaker-Type-SecondaryStatusTransition-StartTime)": number,
         "[Status](API_SecondaryStatusTransition.md#SageMaker-Type-SecondaryStatusTransition-Status)": "string",
         "[StatusMessage](API_SecondaryStatusTransition.md#SageMaker-Type-SecondaryStatusTransition-StatusMessage)": "string"
      }
   ],
   "[StoppingCondition](#SageMaker-DescribeTrainingJob-response-StoppingCondition)": { 
      "[MaxRuntimeInSeconds](API_StoppingCondition.md#SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds)": number
   },
   "[TrainingEndTime](#SageMaker-DescribeTrainingJob-response-TrainingEndTime)": number,
   "[TrainingJobArn](#SageMaker-DescribeTrainingJob-response-TrainingJobArn)": "string",
   "[TrainingJobName](#SageMaker-DescribeTrainingJob-response-TrainingJobName)": "string",
   "[TrainingJobStatus](#SageMaker-DescribeTrainingJob-response-TrainingJobStatus)": "string",
   "[TrainingStartTime](#SageMaker-DescribeTrainingJob-response-TrainingStartTime)": number,
   "[TuningJobArn](#SageMaker-DescribeTrainingJob-response-TuningJobArn)": "string",
   "[VpcConfig](#SageMaker-DescribeTrainingJob-response-VpcConfig)": { 
      "[SecurityGroupIds](API_VpcConfig.md#SageMaker-Type-VpcConfig-SecurityGroupIds)": [ "string" ],
      "[Subnets](API_VpcConfig.md#SageMaker-Type-VpcConfig-Subnets)": [ "string" ]
   }
}
```

## Response Elements<a name="API_DescribeTrainingJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AlgorithmSpecification](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-AlgorithmSpecification"></a>
Information about the algorithm used for training, and algorithm metadata\.   
Type: [AlgorithmSpecification](API_AlgorithmSpecification.md) object

 ** [CreationTime](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-CreationTime"></a>
A timestamp that indicates when the training job was created\.  
Type: Timestamp

 ** [EnableInterContainerTrafficEncryption](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-EnableInterContainerTrafficEncryption"></a>
To encrypt all communications between ML compute instances in distributed training, choose `True`\. Encryption provides greater security for distributed training, but training might take longer\. How long it takes depends on the amount of communication between compute instances, especially if you use a deep learning algorithm in distributed training\.  
Type: Boolean

 ** [EnableNetworkIsolation](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-EnableNetworkIsolation"></a>
If you want to allow inbound or outbound network calls, except for calls between peers within a training cluster for distributed training, choose `True`\. If you enable network isolation for training jobs that are configured to use a VPC, Amazon SageMaker downloads and uploads customer data and model artifacts through the specified VPC, but the training container does not have network access\.  
The Semantic Segmentation built\-in algorithm does not support network isolation\.
Type: Boolean

 ** [FailureReason](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-FailureReason"></a>
If the training job failed, the reason it failed\.   
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [FinalMetricDataList](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-FinalMetricDataList"></a>
A collection of `MetricData` objects that specify the names, values, and dates and times that the training algorithm emitted to Amazon CloudWatch\.  
Type: Array of [MetricData](API_MetricData.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 20 items\.

 ** [HyperParameters](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-HyperParameters"></a>
Algorithm\-specific parameters\.   
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Value Length Constraints: Maximum length of 256\.

 ** [InputDataConfig](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-InputDataConfig"></a>
An array of `Channel` objects that describes each data input channel\.   
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 8 items\.

 ** [LabelingJobArn](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-LabelingJobArn"></a>
The Amazon Resource Name \(ARN\) of the Amazon SageMaker Ground Truth labeling job that created the transform or training job\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:labeling-job/.*` 

 ** [LastModifiedTime](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-LastModifiedTime"></a>
A timestamp that indicates when the status of the training job was last modified\.  
Type: Timestamp

 ** [ModelArtifacts](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-ModelArtifacts"></a>
Information about the Amazon S3 location that is configured for storing model artifacts\.   
Type: [ModelArtifacts](API_ModelArtifacts.md) object

 ** [OutputDataConfig](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-OutputDataConfig"></a>
The S3 path where model artifacts that you configured when creating the job are stored\. Amazon SageMaker creates subfolders for model artifacts\.   
Type: [OutputDataConfig](API_OutputDataConfig.md) object

 ** [ResourceConfig](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-ResourceConfig"></a>
Resources, including ML compute instances and ML storage volumes, that are configured for model training\.   
Type: [ResourceConfig](API_ResourceConfig.md) object

 ** [RoleArn](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-RoleArn"></a>
The AWS Identity and Access Management \(IAM\) role configured for the training job\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$` 

 ** [SecondaryStatus](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-SecondaryStatus"></a>
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

 ** [SecondaryStatusTransitions](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-SecondaryStatusTransitions"></a>
A history of all of the secondary statuses that the training job has transitioned through\.  
Type: Array of [SecondaryStatusTransition](API_SecondaryStatusTransition.md) objects

 ** [StoppingCondition](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-StoppingCondition"></a>
The condition under which to stop the training job\.   
Type: [StoppingCondition](API_StoppingCondition.md) object

 ** [TrainingEndTime](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-TrainingEndTime"></a>
Indicates the time when the training job ends on training instances\. You are billed for the time interval between the value of `TrainingStartTime` and this time\. For successful jobs and stopped jobs, this is the time after model artifacts are uploaded\. For failed jobs, this is the time when Amazon SageMaker detects a job failure\.  
Type: Timestamp

 ** [TrainingJobArn](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-TrainingJobArn"></a>
The Amazon Resource Name \(ARN\) of the training job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:training-job/.*` 

 ** [TrainingJobName](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-TrainingJobName"></a>
 Name of the model training job\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [TrainingJobStatus](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-TrainingJobStatus"></a>
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

 ** [TrainingStartTime](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-TrainingStartTime"></a>
Indicates the time when the training job starts on training instances\. You are billed for the time interval between this time and the value of `TrainingEndTime`\. The start time in CloudWatch Logs might be later than this time\. The difference is due to the time it takes to download the training data and to the size of the training container\.  
Type: Timestamp

 ** [TuningJobArn](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-TuningJobArn"></a>
The Amazon Resource Name \(ARN\) of the associated hyperparameter tuning job if the training job was launched by a hyperparameter tuning job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:hyper-parameter-tuning-job/.*` 

 ** [VpcConfig](#API_DescribeTrainingJob_ResponseSyntax) **   <a name="SageMaker-DescribeTrainingJob-response-VpcConfig"></a>
A [VpcConfig](API_VpcConfig.md) object that specifies the VPC that this training job has access to\. For more information, see [Protect Training Jobs by Using an Amazon Virtual Private Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/train-vpc.html)\.  
Type: [VpcConfig](API_VpcConfig.md) object

## Errors<a name="API_DescribeTrainingJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeTrainingJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeTrainingJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeTrainingJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeTrainingJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeTrainingJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeTrainingJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeTrainingJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeTrainingJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeTrainingJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeTrainingJob) 