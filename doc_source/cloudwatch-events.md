# React to Amazon SageMaker Job Status Changes with CloudWatch Events<a name="cloudwatch-events"></a>

To react to a status change in a Amazon SageMaker training, hyperparameter tuning, or batch transform job, reate a rule in CloudWatch Events that use the **SageMaker Training Job State Change**, **SageMaker Hyperparameter Tuning Job State Change**, or **SageMaker Transform Job State Change** event type as the event source for the rule\. 

Every time the status of a Amazon SageMaker job changes, it triggers an event that CloudWatch Events monitors, and you can create a rule that calls a AWS Lambda function when the status changes\. For information about the status values and meanings for Amazon SageMaker jobs, see the following:
+ [TrainingJobStatus](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeTrainingJob.html#SageMaker-DescribeTrainingJob-response-TrainingJobStatus)
+ [HyperParameterTuningJobStatus](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeHyperParameterTuningJob.html#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus)
+ [TransformJobStatus](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeTransformJob.html#SageMaker-DescribeTransformJob-response-TransformJobStatus)

For information about creating CloudWatch Events rules, see [Creating a CloudWatch Events Rule That Triggers on an Event ](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html) in the *CloudWatch Events User Guide*\. For detailed information about the format of the Amazon SageMaker events that CloudWatch Events monitors, see [Amazon SageMaker Events](https://docs.aws.amazon.com/https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#sagemaker_event_types)\.