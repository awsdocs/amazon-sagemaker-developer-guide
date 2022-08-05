# Supported Frameworks and AWS Regions<a name="distributed-model-parallel-support"></a>

Before using the SageMaker model parallel library, check the supported frameworks and instance types, and determine if there are enough quotas in your AWS account and AWS Region\.

## Supported Frameworks<a name="distributed-model-parallel-supported-frameworks"></a>

The SageMaker model parallel library supports the following deep learning frameworks and is available in AWS Deep Learning Containers \(DLC\) or downloadable as a binary file\.


**PyTorch versions supported by SageMaker and the SageMaker distributed model parallel library**  

| PyTorch version | SageMaker distributed model parallel library version | `smdistributed-modelparallel` integrated DLC image URI | URL of the binary file\*\* | 
| --- | --- | --- | --- | 
| v1\.12\.0\* | smdistributed\-modelparallel==v1\.10\.0 |  `763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.12.0-gpu-py38-cu113-ubuntu20.04-sagemaker`  | https://sagemaker\-distributed\-model\-parallel\.s3\.us\-west\-2\.amazonaws\.com/pytorch\-1\.12\.0/build\-artifacts/2022\-07\-11\-19\-23/smdistributed\_modelparallel\-1\.10\.0\-cp38\-cp38\-linux\_x86\_64\.whl | 
| v1\.11\.0\* | smdistributed\-modelparallel==v1\.10\.0 |  `763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.11.0-gpu-py38-cu113-ubuntu20.04-sagemaker`  | https://sagemaker\-distributed\-model\-parallel\.s3\.us\-west\-2\.amazonaws\.com/pytorch\-1\.11\.0/build\-artifacts/2022\-07\-11\-19\-23/smdistributed\_modelparallel\-1\.10\.0\-cp38\-cp38\-linux\_x86\_64\.whl | 
| v1\.10\.2\* |  smdistributed\-modelparallel==v1\.7\.0 |  `763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.10.2-gpu-py38-cu113-ubuntu20.04-sagemaker`  | \- | 
| v1\.10\.0 |  smdistributed\-modelparallel==v1\.5\.0 |  `763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.10.0-gpu-py38-cu113-ubuntu20.04-sagemaker`  | \- | 
| v1\.9\.1 |  smdistributed\-modelparallel==v1\.4\.0 |  `763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.9.1-gpu-py38-cu111-ubuntu20.04`  | \- | 
| v1\.8\.1\* |  smdistributed\-modelparallel==v1\.6\.0 |  `763104351884.dkr.ecr.<region>.amazonaws.com/pytorch-training:1.8.1-gpu-py36-cu111-ubuntu18.04`  | \- | 

\* The SageMaker distributed model parallel library v1\.6\.0 and later provides extended features for PyTorch\. For more information, see [Extended Features of the SageMaker Model Parallel Library for PyTorch](model-parallel-extended-features-pytorch.md)\.

\*\* The URLs of the binary files are for installing the SageMaker distributed model parallelism library in custom containers\. For more information, see [Create Your Own Docker Container with the SageMaker Distributed Model Parallel Library](model-parallel-sm-sdk.md#model-parallel-bring-your-own-container)\.


**TensorFlow versions supported by SageMaker and the SageMaker distributed model parallel library**  

| TensorFlow version | SageMaker distributed model parallel library version | `smdistributed-modelparallel` integrated DLC image URI | 
| --- | --- | --- | 
| v2\.6\.0 | smdistributed\-modelparallel==v1\.4\.0 | 763104351884\.dkr\.ecr\.<region>\.amazonaws\.com/tensorflow\-training:2\.6\.0\-gpu\-py38\-cu112\-ubuntu20\.04 | 
| v2\.5\.1 | smdistributed\-modelparallel==v1\.4\.0  | 763104351884\.dkr\.ecr\.<region>\.amazonaws\.com/tensorflow\-training:2\.5\.1\-gpu\-py37\-cu112\-ubuntu18\.04  | 

**Hugging Face Transformers versions supported by SageMaker and the SageMaker distributed data parallel library**

The AWS Deep Learning Containers for Hugging Face use the SageMaker Training Containers for PyTorch and TensorFlow as their base images\. To look up the Hugging Face Transformers library versions and paired PyTorch and TensorFlow versions, see the latest [Hugging Face Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#huggingface-training-containers) and the [Prior Hugging Face Container Versions](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#prior-hugging-face-container-versions)\.

**Note**  
To check the latest updates and release history of the library, see the [SageMaker Distributed Model Parallel Release Notes](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_release_notes/smd_model_parallel_change_log.html) in the *SageMaker Python SDK documentation*\.

## AWS Regions<a name="distributed-model-parallel-availablity-zone"></a>

The SageMaker data parallel library is available in all of the AWS Regions where the [AWS Deep Learning Containers for SageMaker](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-framework-containers-sm-support-only) are in service\. For more information, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#available-deep-learning-containers-images)\.