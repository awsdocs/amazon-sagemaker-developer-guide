# Run a SageMaker Distributed Training Job with Data Parallelism<a name="data-parallel-modify-sdp"></a>

SageMaker's distributed data parallel library APIs are designed for ease of use and to provide seamless integration with existing distributed training toolkits\.
+ **SageMaker Python SDK with the library API** – In most cases, all you have to change in your training script is the data parallel library import statements\. Swap these out with the SageMaker data parallel library equivalents\.
+ **Focus on your model training without infrastructure management** – When training a deep learning model with the library on SageMaker, you can focus on writing your training script and model training\. You can run a training job using estimator classes provided by the SageMaker Python SDK\. The estimator classes help prepare ML instances, load datasets from specified data resources, submit the training job using your training script, and shut down the instances after the training job is completed\.

To begin, you need to adapt TensorFlow or PyTorch training scripts to use the library\. The following topics provide instructions on how to modify your training script\.

**Topics**
+ [Step 1: Modify Your Own Training Script](data-parallel-modify-sdp-select-framework.md)
+ [Step 2: Launch a SageMaker Distributed Training Job Using the SageMaker Python SDK](data-parallel-use-api.md)