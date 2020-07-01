# Managed Spot Training in Amazon SageMaker<a name="model-managed-spot-training"></a>

Amazon SageMaker makes it easy to train machine learning models using managed Amazon EC2 Spot instances\. Managed spot training can optimize the cost of training models up to 90% over on\-demand instances\. Amazon SageMaker manages the Spot interruptions on your behalf\. 

Managed Spot Training uses Amazon EC2 Spot instance to run training jobs instead of on\-demand instances\. You can specify which training jobs use spot instances and a stopping condition that specifies how long Amazon SageMaker waits for a job to run using Amazon EC2 Spot instances\. Metrics and logs generated during training runs are available in CloudWatch\. 

Spot instances can be interrupted, causing jobs to take longer to start or finish\. You can configure your managed spot training job to use checkpoints\. Amazon SageMaker copies checkpoint data from a local path to Amazon S3\. When the job is restarted, Amazon SageMaker copies the data from Amazon S3 back into the local path\. The training can then resume from the last checkpoint instead of restarting\. For more information about checkpointing, see [Use Checkpoints in Amazon SageMaker](model-checkpoints.md)\.

**Note**  
Unless your training job will complete quickly, we recommend you use checkpointing with managed spot training\. SageMaker built\-in algorithms and marketplace algorithms that do not checkpoint are currently limited to a `MaxWaitTimeInSeconds` of 3600 seconds \(60 minutes\)\. 

**Topics**
+ [Using Managed Spot Training](#model-managed-spot-training-using)
+ [Managed Spot Training Lifecycle](#model-managed-spot-training-status)

## Using Managed Spot Training<a name="model-managed-spot-training-using"></a>

To use managed spot training, create a training job\. Set `EnableManagedSpotTraining` to `True` and specify the `MaxWaitTimeInSeconds`\. `MaxWaitTimeInSeconds` must be larger than `MaxRuntimeInSeconds`\. For more information about creating a training job, see [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html)\. 

You can calculate the savings from using managed spot training using the formula `(1 - BillableTimeInSeconds / TrainingTimeInSeconds) * 100`\. For example, if `BillableTimeInSeconds` is 100 and `TrainingTimeInSeconds` is 500, the savings is 80%\.

## Managed Spot Training Lifecycle<a name="model-managed-spot-training-status"></a>

You can monitor a training job using `TrainingJobStatus` and `SecondaryStatus` returned by [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html)\. The list below shows how `TrainingJobStatus` and `SecondaryStatus` values change depending on the training scenario:
+ **Spot instances acquired with no interruption during training**

  1. `InProgress`: `Starting`↠ `Downloading` ↠ `Training` ↠ `Uploading`
+ **Spot instances interrupted once\. Later, enough spot instances were acquired to finish the training job\.**

  1. `InProgress`: `Starting` ↠ `Downloading` ↠ `Training` ↠ `Interrupted` ↠ `Starting` ↠ `Downloading` ↠ `Training` ↠ `Uploading` 
+ **Spot instances interrupted twice and `MaxWaitTimeInSeconds` exceeded\.**

  1. `InProgress`: `Starting` ↠ `Downloading` ↠ `Training` ↠ `Interrupted` ↠ `Starting` ↠ `Downloading` ↠ `Training` ↠ `Interrupted` ↠ `Downloading` ↠ `Training` 

  1. `Stopping`: `Stopping` 

  1. `Stopped`: `MaxWaitTimeExceeded` 
+ **Spot instances were never launched\.**

  1. `InProgress`: `Starting` 

  1. `Stopping`: `Stopping` 

  1. `Stopped`: `MaxWaitTimeExceeded` 