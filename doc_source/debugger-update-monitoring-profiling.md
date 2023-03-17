# Updating Debugger System Monitoring and Framework Profiling Configuration while a Training Job is Running<a name="debugger-update-monitoring-profiling"></a>

If you want to activate or update the Debugger monitoring configuration for a training job that is currently running, use the following SageMaker estimator extension methods:
+ To activate Debugger system monitoring for a running training job and receive a Debugger profiling report, use the following:

  ```
  estimator.enable_default_profiling()
  ```

  When you use the `enable_default_profiling` method, Debugger initiates the default system monitoring and the `ProfileReport` built\-in rule, which generates a comprehensive profiling report at the end of the training job\. This method can be called only if the current training job is running without both Debugger monitoring and profiling\.

  For more information, see [estimator\.enable\_default\_profiling](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator.enable_default_profiling) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.
+ To update system monitoring configuration, use the following:

  ```
  estimator.update_profiler(
      system_monitor_interval_millis=500
  )
  ```

  For more information, see [estimator\.update\_profiler](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator.update_profiler) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.