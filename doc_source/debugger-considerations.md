# Considerations for Amazon SageMaker Debugger<a name="debugger-considerations"></a>

Consider the following when using Amazon SageMaker Debugger\.

## Considerations for Distributed Training<a name="w1151aac24c16c42c16b5"></a>
+ **Horovod support** 
  + For debugging – Debugger does not support Horovod distributed training for Keras\.
  + For profiling – Debugger does not support Horovod distributed training for Keras and MXNet\.
+ **Parameter Server support** – Parameter server\-based distributed training is not supported\.

## Considerations for Monitoring and Profiling<a name="w1151aac24c16c42c16b7"></a>
+ For AWS TensorFlow, data loader metrics cannot be collected using the default `local_path` setting of the `FrameworkProfile` class\. The path has to be manually configured ending in `"/"`\. For example:

  ```
  FrameworkProfile(local_path="/opt/ml/output/profiler/")
  ```
+ For AWS TensorFlow, the data loader profiling configuration cannot be updated while training job is running\.
+ For AWS TensorFlow, `NoneType` error might occur when you use analysis tools and notebook examples with TensorFlow 2\.3 training jobs and the detailed profiling option\.
+ Python profiling and detailed profiling are only supported for Keras API\.