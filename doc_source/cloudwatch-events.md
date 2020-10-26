# React to Amazon SageMaker Job Status Changes with CloudWatch Events<a name="cloudwatch-events"></a>

Amazon CloudWatch Events monitors status changes in SageMaker labeling, training, hyperparameter tuning, and inference jobs\. Every time the job status changes, CloudWatch Events invokes *Targets* to react to the change\. For example, a CloudWatch Events rule can monitor a SageMaker training job and trigger an AWS Lambda function to automatically terminate the job when its status changes\.

To create a CloudWatch Events rule, open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/) and follow instructions provided at [Creating a CloudWatch Events Rule That Triggers on an Event](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html)\. You can set an *Event Pattern* to monitor a SageMaker job and a Target to invoke\. The CloudWatch console assists you to generate rules providing a SageMaker Event Pattern preview code depending on your choice of **Event Type**\.

For more information about the status values and their meanings for Amazon SageMaker jobs, see the following links:
+ [ `AlgorithmStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAlgorithm.html#sagemaker-DescribeAlgorithm-response-AlgorithmStatus)
+ [ `EndpointStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html#sagemaker-DescribeEndpoint-response-EndpointStatus)
+ [ `HyperParameterTuningJobStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html#sagemaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus)
+ [ `LabelingJobStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeLabelingJob.html#sagemaker-DescribeLabelingJob-response-LabelingJobStatus)
+ [ `ModelPackageStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeModelPackage.html#sagemaker-DescribeModelPackage-response-ModelPackageStatus)
+ [ `NotebookInstanceStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeNotebookInstance.html#sagemaker-DescribeNotebookInstance-response-NotebookInstanceStatus)
+ [ `ProcessingJobStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html#sagemaker-DescribeProcessingJob-response-ProcessingJobStatus)
+ [ `TrainingJobStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html#sagemaker-DescribeTrainingJob-response-TrainingJobStatus)
+ [ `TransformJobStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTransformJob.html#sagemaker-DescribeTransformJob-response-TransformJobStatus)

For more information about the format of the Amazon SageMaker events that CloudWatch Events monitors, see [Amazon SageMaker Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#sagemaker_event_types)\.

To learn more about creating a Lambda function, see [ Create a Lambda function with the console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html)\.