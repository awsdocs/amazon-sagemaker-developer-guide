# Monitor the Progress of a Hyperparameter Tuning Job<a name="automatic-model-tuning-monitor"></a>

To monitor the progress of a hyperparameter tuning job and the training jobs that it launches, use the Amazon SageMaker console\.

**Topics**
+ [View the Status of the Hyperparameter Tuning Job](#automatic-model-tuning-monitor-tuning)

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