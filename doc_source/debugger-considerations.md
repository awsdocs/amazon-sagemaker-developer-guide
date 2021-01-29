# Considerations for Amazon SageMaker Debugger<a name="debugger-considerations"></a>

Consider the following when using Amazon SageMaker Debugger\.

## Considerations for Distributed Training<a name="w1199aac24c16c43c16b5"></a>
+ **Horovod support** 
  + For debugging – Debugger does not support Horovod distributed training for Keras\.
  + For profiling – Debugger does not support Horovod distributed training for Keras and MXNet\.
+ **Parameter Server support** – Parameter server\-based distributed training is not supported\.

## Considerations for Monitoring and Profiling<a name="w1199aac24c16c43c16b7"></a>
+ For AWS TensorFlow, data loader metrics cannot be collected using the default `local_path` setting of the `FrameworkProfile` class\. The path has to be manually configured ending in `"/"`\. For example:

  ```
  FrameworkProfile(local_path="/opt/ml/output/profiler/")
  ```
+ For AWS TensorFlow, the data loader profiling configuration cannot be updated while training job is running\.
+ For AWS TensorFlow, `NoneType` error might occur when you use analysis tools and notebook examples with TensorFlow 2\.3 training jobs and the detailed profiling option\.
+ Python profiling and detailed profiling are only supported for Keras API\.
+ To access the deep profiling feature for TensorFlow and PyTorch, currently you need to specify the latest AWS deep learning container images with CUDA 11\. For example, you must specify the specific image URI in the TensorFlow and PyTorch estimator as follows:
  + For TensorFlow

    ```
    image_uri = f"763104351884.dkr.ecr.{region}.amazonaws.com/tensorflow-training:2.3.1-gpu-py37-cu110-ubuntu18.04"
    ```
  + For PyTorch

    ```
    image_uri = f"763104351884.dkr.ecr.{region}.amazonaws.com/pytorch-training:1.6.0-gpu-py36-cu110-ubuntu18.04"
    ```