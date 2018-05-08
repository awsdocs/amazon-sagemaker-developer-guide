# Logging Amazon SageMaker with Amazon CloudWatch<a name="logging-cloudwatch"></a>

To help you debug your training jobs, endpoints, and notebook instance lifecycle configurations, anything an algorithm container, a model container, or a notebook instance lifecycle configuration sends to `stdout` or `stderr` is also sent to Amazon CloudWatch Logs\. In addition to debugging, you can use these for progress analysis\.

**Logs**


| Log Group Name | Log Stream Name | 
| --- | --- | 
| /aws/sagemaker/TrainingJobs |  `[training-job-name]/algo-[instance-number-in-cluster]-[epoch_timestamp]`  | 
| /aws/sagemaker/Endpoints/\[EndpointName\] |  `[production-variant-name]/[instance-id]`  | 
| /aws/sagemaker/NotebookInstances |  `[notebook-instance-name]/[LifecycleConfigHook]`  | 