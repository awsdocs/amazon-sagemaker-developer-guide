# Track and set completion criteria for your tuning job<a name="automatic-model-tuning-progress"></a>

You can use completion criteria to instruct Automatic model tuning \(AMT\) to stop your tuning job if certain conditions are met\. With these conditions, you can set a minimum model performance or maximum number of training jobs that don’t improve when evaluated against the objective metric\. You can also track the progress of your tuning job and decide to let it continue or to stop it manually\. This guide shows you how to set completion criteria, check the progress of and stop your tuning job manually\.

## Set completion criteria for your tuning job<a name="automatic-model-tuning-progress-completion"></a>

During hyperparameter optimization, a tuning job will launch several training jobs inside a loop\. The tuning job will do the following\. 
+ Check your training jobs for completion and update statistics accordingly
+ Decide what combination of hyperparameters to evaluate next\.

AMT will continuously check the training jobs that were launched from your tuning job to update statistics\. These statistics include tuning job runtime and best training job\. Then, AMT determines whether it should stop the job according to your completion criteria\. You can also check these statistics and stop your job manually\. For more information about stopping a job manually, see the [Stopping your tuning job manually](#automatic-model-tuning-progress-stop) section\.

As an example, if your tuning job meets your objective, you can stop tuning early to conserve resources or ensure model quality\. AMT checks your job performance against your completion criteria and stops the tuning job if any have been met\. 

You can specify the following kinds of completion criteria:
+ `MaxNumberOfTrainingJobs` – The maximum number of training jobs to be run before tuning is stopped\.
+ `MaxNumberOfTrainingJobsNotImproving` – The maximum number of training jobs that do not improve performance against the objective metric from the current best training job\. As an example, if the best training job returned an objective metric that had an accuracy of `90%`, and `MaxNumberOfTrainingJobsNotImproving` is set to `10`\. In this example, tuning will stop after `10` training jobs fail to return an accuracy higher than `90`%\.
+ `MaxRuntimeInSeconds` – The upper limit of wall clock time in seconds of how long a tuning job can run\.
+ `TargetObjectiveMetricValue` – The value of the objective metric against which the tuning job is evaluated\. Once this value is met, AMT stops the tuning job\.
+ `CompleteOnConvergence` – A flag to stop tuning after an internal algorithm determines that the tuning job is unlikely to improve more than 1% over the objective metric from the best training job\.

### Selecting completion criteria<a name="automatic-model-tuning-progress-completion-how"></a>

You can choose one or multiple completion criteria to stop your hyperparameter tuning job after a condition has been meet\. The following instructions show you how to select completion criteria and how to decide which is the most appropriate for your use case\.
+ Use `MaxNumberOfTrainingJobs` in the [ResourceLimits](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResourceLimits.html) API to set an upper limit for the number of training jobs that can be run before your tuning job is stopped\. Start with a large number and adjust it based on model performance against your tuning job objective\. Most users input values of around `50` or more training jobs to find an optimal hyperparameter configuration\. Users looking for higher levels of model performance will use `200` or more training jobs\.
+ Use `MaxNumberOfTrainingJobsNotImproving` in the [BestObjectiveNotImproving](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_BestObjectiveNotImproving.html) API field to stop training if model performance fails to improve after a specified number of jobs\. Model performance is evaluated against an objective function\. After the `MaxNumberOfTrainingJobsNotImproving` is met, AMT will stop the tuning job\. Tuning jobs tend to make the most progress in the beginning of the job\. Improving model performance against an objective function will require a larger number of training jobs towards the end of tuning\. Select a value for `MaxNumberOfTrainingJobsNotImproving` by checking the performance of similar training jobs against your objective metric\.
+ Use `MaxRuntimeInSeconds` in the [ResourceLimits](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResourceLimits.html) API to set an upper limit for the amount of wall clock time that the tuning job may take\. Use this field to meet a deadline by which the tuning job must complete or to limit compute resources\.

  To get an estimated total compute time in seconds for a tuning job, use the following formula:

  Estimated max compute time in seconds= `MaxRuntimeInSeconds` \* `MaxParallelTrainingJobs` \* `MaxInstancesPerTrainingJob` 
**Note**  
The actual duration of a tuning job may deviate slightly from the value specified in this field\.
+ Use `TargetObjectiveMetricValue` in the [TuningJobCompletionCriteria](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TuningJobCompletionCriteria.html) API to stop your tuning job\. You stop the tuning job after any training job that is launched by the tuning job reaches this objective metric value\. Use this field if your use case depends on reaching a specific performance level, rather than spending compute resources to find the best possible model\.
+ Use `CompleteOnConvergence` in the [TuningJobCompletionCriteria](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TuningJobCompletionCriteria.html) API to stop a tuning job after AMT has detected that the tuning job has converged, and is unlikely to make further significant progress\. Use this field when it is not clear what values for any of the other completion criteria should be used\. AMT determines convergence based on an algorithm developed and tested on a wide range of diverse benchmarks\. A tuning job is defined to have converged when none of the training jobs return significant improvement \(1% or less\)\. Improvement is measured against the objective metric returned by the highest performing job, so far\.

### Combining different completion criteria<a name="automatic-model-tuning-progress-completion-combine"></a>

You can also combine any of the different completion criteria in the same tuning job\. AMT will stop the tuning job when any one of the completion criteria is met\. For example, if you want to tune your model until it meets an objective metric, but don't want to keep tuning if your job has converged, use the following guidance\.
+ Specify `TargetObjectiveMetricValue` in the [TuningJobCompletionCriteria](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TuningJobCompletionCriteria.html) API to set a target objective metrics value to reach\.
+ Set [CompleteOnConvergence](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ConvergenceDetected.html) to `Enabled` to stop a tuning job if AMT has determined that model performance is unlikely to improve\.

## Track tuning job progress<a name="automatic-model-tuning-progress-track"></a>

You can use the `DescribeHyperParameterTuningJob` API to track the progress of your tuning job at any time while it is running\. You don't have to specify completion criteria to obtain tracking information for your tuning job\. Use the following fields to obtain statistics about your tuning job\.
+ [BestTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html#sagemaker-DescribeHyperParameterTuningJob-response-BestTrainingJob) – An object that describes the best training job obtained so far, evaluated against your objective metric\. Use this field to check your current model performance and the value of the objective metric of this best training job\.
+ [ObjectiveStatusCounters](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html#sagemaker-DescribeHyperParameterTuningJob-response-ObjectiveStatusCounters) – An object that specifies the total number of training jobs completed in a tuning job\. To estimate average duration of a tuning job, use `ObjectiveStatusCounters` and the total runtime of a tuning job\. You can use the average duration to estimate how much longer your tuning job will run\.
+ `ConsumedResources` – The total resources, such as `RunTimeInSeconds`, consumed by your tuning job\. Compare `ConsumedResources`, found in the [DescribeHyperParameterTuningJob ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html) API, against `BestTrainingJob` in the same API\. You can also compare `ConsumedResources` against the response from the [ListTrainingJobsForHyperParameterTuningJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListTrainingJobsForHyperParameterTuningJob.html) API to assess if your tuning job is making satisfactory progress given the resources being consumed\.
+ [TuningJobCompletionDetails](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HyperParameterTuningJobCompletionDetails.html) – Tuning job completion information that includes the following:
  + The timestamp of when convergence is detected if the job has converged\.
  + The number of training jobs that have not improved model performance\. Model performance is evaluated against the objective metric from the best training job\.

  Use the tuning job completion criteria to assess how likely your tuning job is to improve your model performance\. Model performance is evaluated against the best objective metric if it ran to completion\.

## Stopping your tuning job manually<a name="automatic-model-tuning-progress-stop"></a>

You can determine if you should let the tuning job run until it completes or if you should stop the tuning job manually\. To determine this, use the information returned by the parameters in the `DescribeHyperParameterTuningJob` API, as shown in the previous **Tracking tuning job progress** section\. As an example, if your model performance does not improve after several training jobs complete, you may choose to stop the tuning job\. Model performance is evaluated against the best objective metric\.

To stop the tuning job manually, use the [StopHyperParameterTuningJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopHyperParameterTuningJob.html) API and provide the name of the tuning job to be stopped\.