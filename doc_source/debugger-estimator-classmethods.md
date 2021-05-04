# Useful SageMaker Estimator Classmethods for Debugger<a name="debugger-estimator-classmethods"></a>

The following estimator class methods are useful for accessing your SageMaker training job information and retrieving output paths of training data collected by Debugger\. The following methods are executable after you initiate a training job with the `estimator.fit()` method\.
+ To check the base S3 bucket URI of a SageMaker training job:

  ```
  estimator.output_path
  ```
+ To check the base job name of a SageMaker training job:

  ```
  estimator.latest_training_job.job_name
  ```
+ To see a full `CreateTrainingJob` API operation configuration of a SageMaker training job:

  ```
  estimator.latest_training_job.describe()
  ```
+ To check a full list of the Debugger rules while a SageMaker training job is running:

  ```
  estimator.latest_training_job.rule_job_summary()
  ```
+ To check the S3 bucket URI where the model parameter data \(output tensors\) are saved:

  ```
  estimator.latest_job_debugger_artifacts_path()
  ```
+ To check the S3 bucket URI at where the model performance data \(system and framework metrics\) are saved:

  ```
  estimator.latest_job_profiler_artifacts_path()
  ```
+ To check the Debugger rule configuration for debugging output tensors:

  ```
  estimator.debugger_rule_configs
  ```
+ To check the list of the Debugger rules for debugging while a SageMaker training job is running:

  ```
  estimator.debugger_rules
  ```
+ To check the Debugger rule configuration for monitoring and profiling system and framework metrics:

  ```
  estimator.profiler_rule_configs
  ```
+ To check the list of the Debugger rules for monitoring and profiling while a SageMaker training job is running:

  ```
  estimator.profiler_rules
  ```

For more information about the SageMaker estimator class and its methods, see [Estimator API](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.