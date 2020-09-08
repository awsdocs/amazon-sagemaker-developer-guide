# Monitor the Progress of a Hyperparameter Tuning Job<a name="automatic-model-tuning-monitor"></a>

To monitor the progress of a hyperparameter tuning job and the training jobs that it launches, use the Amazon SageMaker console\.

**Topics**
+ [View the Status of the Hyperparameter Tuning Job](#automatic-model-tuning-monitor-tuning)
+ [View the Status of the Training Jobs](#automatic-model-tuning-monitor-training)
+ [View the Best Training Job](#automatic-model-tuning-best-training-job)

## View the Status of the Hyperparameter Tuning Job<a name="automatic-model-tuning-monitor-tuning"></a>

**To view the status of the hyperparameter tuning job**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Hyperparameter tuning jobs**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/console-tuning-jobs.png)

1. In the list of hyperparameter tuning jobs, check the status of the hyperparameter tuning job you launched\. A tuning job can be:
   + `Completed`—The hyperparameter tuning job successfully completed\.
   + `InProgress`—The hyperparameter tuning job is in progress\. One or more training jobs are still running\.
   + `Failed`—The hyperparameter tuning job failed\.
   + `Stopped`—The hyperparameter tuning job was manually stopped before it completed\. All training jobs that the hyperparameter tuning job launched are stopped\.
   + `Stopping`—The hyperparameter tuning job is in the process of stopping\.

## View the Status of the Training Jobs<a name="automatic-model-tuning-monitor-training"></a>

**To view the status of the training jobs that the hyperparameter tuning job launched**

1. In the list of hyperparameter tuning jobs, choose the job that you launched\.

1. Choose **Training jobs**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/hyperparameter-training-jobs.png)

1. View the status of each training job\. To see more details about a job, choose it in the list of training jobs\. To view a summary of the status of all of the training jobs that the hyperparameter tuning job launched, see **Training job status counter**\.

   A training job can be:
   + `Completed`—The training job successfully completed\.
   + `InProgress`—The training job is in progress\.
   + `Stopped`—The training job was manually stopped before it completed\.
   + `Failed (Retriable)`—The training job failed, but can be retried\. A failed training job can be retried only if it failed because an internal service error occurred\.
   + `Failed (Non-retriable)`—The training job failed and can't be retried\. A failed training job can't be retried when a client error occurs\.

## View the Best Training Job<a name="automatic-model-tuning-best-training-job"></a>

A hyperparameter tuning job uses the objective metric that each training job returns to evaluate training jobs\. While the hyperparameter tuning job is in progress, the best training job is the one that has returned the best objective metric so far\. After the hyperparameter tuning job is complete, the best training job is the one that returned the best objective metric\.

To view the best training job, choose **Best training job**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/best-training-job.png)

To deploy the best training job as a model that you can host at a SageMaker endpoint, choose **Create model**\.

### Next Step<a name="automatic-model-tuning-ex-next-cleanup"></a>

[Clean up](automatic-model-tuning-ex-cleanup.md)