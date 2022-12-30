# Stop a Training Job in SageMaker Studio<a name="studio-tasks-stop-training-job"></a>

You can stop a training job with the Amazon SageMaker Studio UI\. When you stop a training job, its status changes to `Stopping` at which time billing ceases\. An algorithm can delay termination in order to save model artifacts after which the job status changes to `Stopped`\. For more information, see the [stop\_training\_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.stop_training_job) method in the AWS SDK for Python \(Boto3\)\.

**To stop a training job**

1. Follow the [View, search, and compare experiment runs](experiments-view-compare.md) procedure on this page until you open the **Describe Trial Component** tab\.

1. At the upper\-right side of the tab, choose **Stop training job**\. The **Status** at the top left of the tab changes to **Stopped**\.

1. To view the training time and billing time, choose **AWS Settings**\.