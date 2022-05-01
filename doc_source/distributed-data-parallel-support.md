# Supported Frameworks, AWS Regions, and Instances Types<a name="distributed-data-parallel-support"></a>

Before using the SageMaker data parallel library, check what are the supported ML frameworks and instance types and if there are enough quotas in your AWS account and AWS Region\.

## Supported Frameworks<a name="distributed-data-parallel-supported-frameworks"></a>

The SageMaker data parallel library supports the following deep learning frameworks and is available in AWS Deep Learning Containers \(DLC\) or downloadable as a binary file\.

**PyTorch versions supported by SageMaker and the SageMaker distributed data parallel library**


| PyTorch version | SageMaker distributed data parallel library version | `smdistributed-dataparallel` integrated image URI | URL of the binary file\*\* | 
| --- | --- | --- | --- | 
| v1\.10\.2 |  smdistributed\-dataparallel==v1\.4\.0\* | 763104351884\.dkr\.ecr\.<region>\.amazonaws\.com/pytorch\-training:1\.10\.2\-gpu\-py38\-cu113\-ubuntu20\.04\-sagemaker | https://smdataparallel\.s3\.amazonaws\.com/binary/pytorch/1\.10\.2/cu113/2022\-02\-18/smdistributed\_dataparallel\-1\.4\.0\-cp38\-cp38\-linux\_x86\_64\.whl | 
| v1\.9\.1 |  smdistributed\-dataparallel==v1\.2\.0  |  763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/pytorch\-training:1\.9\.1\-gpu\-py38\-cu111\-ubuntu20\.04  | https://smdataparallel\.s3\.amazonaws\.com/binary/pytorch/1\.9\.0/cu111/2021\-08\-13/smdistributed\_dataparallel\-1\.2\.0\-cp38\-cp38\-linux\_x86\_64\.whl | 
| v1\.8\.1 | smdistributed\-dataparallel==v1\.2\.3  | 763104351884\.dkr\.ecr\.<region>\.amazonaws\.com/pytorch\-training:1\.8\.1\-gpu\-py36\-cu111\-ubuntu18\.04 | https://smdataparallel\.s3\.amazonaws\.com/binary/pytorch/1\.8\.1/cu111/2021\-12\-13/smdistributed\_dataparallel\-1\.2\.3\-cp36\-cp36m\-linux\_x86\_64\.whl | 
| v1\.7\.1 | smdistributed\-dataparallel==v1\.0\.0  |  763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/pytorch\-training:1\.7\.1\-gpu\-py36\-cu110\-ubuntu18\.04  | https://smdataparallel\.s3\.amazonaws\.com/binary/pytorch/1\.7\.1/cu110/2021\-01\-26/smdistributed\_dataparallel\-1\.0\.0\-cp36\-cp36m\-linux\_x86\_64\.whl | 
| v1\.6\.0 | smdistributed\-dataparallel==v1\.0\.0  |  763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/pytorch\-training:1\.6\.0\-gpu\-py36\-cu110\-ubuntu18\.04  | https://smdataparallel\.s3\.amazonaws\.com/binary/pytorch/1\.6\.0/cu110/2021\-01\-14/smdistributed\_dataparallel\-1\.0\.0\-cp36\-cp36m\-linux\_x86\_64\.whl | 

\* The SageMaker distributed data parallelism library v1\.4\.0 and later works as a backend of PyTorch distributed\. In accordance with the change, the following [smdistributed APIs](https://sagemaker.readthedocs.io/en/stable/api/training/sdp_versions/latest/smd_data_parallel_pytorch.html#pytorch-api) for the PyTorch distributed package are deprecated\.
+ `smdistributed.dataparallel.torch.distributed` is deprecated\. Use the [torch\.distributed](https://pytorch.org/docs/stable/distributed.html) package instead\.
+ `smdistributed.dataparallel.torch.parallel.DistributedDataParallel` is deprecated\. Use the [torch\.nn\.parallel\.DistributedDataParallel](https://pytorch.org/docs/stable/generated/torch.nn.parallel.DistributedDataParallel.html) API instead\.

  If you need to use the previous versions of the library \(v1\.3\.0 or before\), see the [archived SageMaker distributed data parallel library documentation](https://sagemaker.readthedocs.io/en/stable/api/training/sdp_versions/latest.html#documentation-archive) in the *SageMaker Python SDK documentation*\.

\*\* The URLs of the binary files are for installing the SageMaker distributed data parallelism library in custom containers\. For more information, see [Create Your Own Docker Container with the SageMaker Distributed Data Parallel Library](data-parallel-use-api.md#data-parallel-bring-your-own-container)\.

**TensorFlow versions supported by SageMaker and the SageMaker distributed data parallel library**


| TensorFlow version | SageMaker distributed data parallel library version | `smdistributed-dataparallel` integrated image URI | 
| --- | --- | --- | 
| 2\.8\.0 |  smdistributed\-dataparallel==v1\.3\.0  | 763104351884\.dkr\.ecr\.<region>\.amazonaws\.com/tensorflow\-training:2\.8\.0\-gpu\-py39\-cu112\-ubuntu20\.04\-sagemaker | 
| 2\.7\.1 |  smdistributed\-dataparallel==v1\.3\.0  |  763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/tensorflow\-training:2\.7\.1\-gpu\-py38\-cu112\-ubuntu20\.04\-sagemaker  | 
| 2\.6\.2 | smdistributed\-dataparallel==v1\.2\.1  |  763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/tensorflow\-training:2\.6\.2\-gpu\-py38\-cu112\-ubuntu20\.04  | 
| 2\.5\.1 | smdistributed\-dataparallel==v1\.2\.1  |  763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/tensorflow\-inference:2\.5\.1\-gpu\-py37\-cu112\-ubuntu18\.04  | 
| 2\.4\.1 | smdistributed\-dataparallel==v1\.2\.0  |  763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/tensorflow\-training:2\.4\.1\-gpu\-py37\-cu110\-ubuntu18\.04  | 
| 2\.3\.2 | smdistributed\-dataparallel==v1\.0\.0  |  763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/tensorflow\-training:2\.3\.2\-gpu\-py37\-cu110\-ubuntu18\.04  | 

**Hugging Face Transformers versions supported by SageMaker and the SageMaker distributed data parallel library**

The AWS Deep Learning Containers for Hugging Face use the SageMaker Training Containers for PyTorch and TensorFlow as their base images\. To look up the Hugging Face Transformers library versions and paired PyTorch and TensorFlow versions, see the latest [Hugging Face Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#huggingface-training-containers) and the [Prior Hugging Face Container Versions](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#prior-hugging-face-container-versions)\.

## AWS Regions<a name="distributed-data-parallel-availablity-zone"></a>

The SageMaker data parallel library is available in all of the AWS Regions where the [AWS Deep Learning Containers for SageMaker](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-framework-containers-sm-support-only) are in service\. For more information, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#available-deep-learning-containers-images)\.

## Supported Instance Types<a name="distributed-data-parallel-supported-instance-types"></a>

The SageMaker data parallel library requires one of the following ML instance types\.


| Instance type | 
| --- | 
| ml\.p3\.16xlarge | 
| ml\.p3dn\.24xlarge  | 
| ml\.p4d\.24xlarge | 

For specs of the instance types, see the **Accelerated Computing** section in the [Amazon EC2 Instance Types page](http://aws.amazon.com/ec2/instance-types/)\. For information about instance pricing, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

If you encountered an error message similar to the following, follow the instructions at [Request a service quota increase for SageMaker resources](https://docs.aws.amazon.com/sagemaker/latest/dg/regions-quotas.html#service-limit-increase-request-procedure)\.

```
ResourceLimitExceeded: An error occurred (ResourceLimitExceeded) when calling
the CreateTrainingJob operation: The account-level service limit 'ml.p3dn.24xlarge
for training job usage' is 0 Instances, with current utilization of 0 Instances
and a request delta of 1 Instances.
Please contact AWS support to request an increase for this limit.
```