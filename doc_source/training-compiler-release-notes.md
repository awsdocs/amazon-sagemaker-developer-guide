# Amazon SageMaker Training Compiler Release Notes<a name="training-compiler-release-notes"></a>

See the following release notes to track the latest updates for Amazon SageMaker Training Compiler\.

## SageMaker Training Compiler Release Notes: September 1, 2022<a name="training-compiler-release-notes-20220825"></a>

**Currency Updates**
+ Added support for Hugging Face Transformers v4\.21\.1 with PyTorch v1\.11\.0\.

**Improvements**
+ Implemented a new distributed training launcher mechanism to activate SageMaker Training Compiler for Hugging Face Transformer models with PyTorch\. To learn more, see [Run PyTorch Training Jobs with SageMaker Training Compiler for Distributed Training](training-compiler-enable-pytorch.md#training-compiler-estimator-pytorch-distributed)\.
+ Integrated with EFA to improve the collective communication in distributed training\.
+ Added support for G5 instances for PyTorch training jobs\. For more information, see [Supported Frameworks, AWS Regions, Instance Types, and Tested Models](training-compiler-support.md)\.

**Migration to AWS Deep Learning Containers**

This release passed benchmark testing and is migrated to the following AWS Deep Learning Container:
+ [HuggingFace v4\.21\.1 with PyTorch v1\.11\.0](https://github.com/aws/deep-learning-containers/releases/tag/v1.0-trcomp-hf-4.21.1-pt-1.11.0-tr-gpu-py38)

  ```
  763104351884.dkr.ecr.us-west-2.amazonaws.com/huggingface-pytorch-trcomp-training:1.11.0-transformers4.21.1-gpu-py38-cu113-ubuntu20.04
  ```

  To find a complete list of the prebuilt containers with Amazon SageMaker Training Compiler, see [Supported Frameworks, AWS Regions, Instance Types, and Tested Models](training-compiler-support.md)\.

## SageMaker Training Compiler Release Notes: June 14, 2022<a name="training-compiler-release-notes-20220614"></a>

**New Features**
+ Added support for TensorFlow v2\.9\.1\. SageMaker Training Compiler fully supports compiling TensorFlow modules \(`tf.*`\) and TensorFlow Keras modules \(`tf.keras.*`\)\.
+ Added support for custom containers created by extending AWS Deep Learning Containers for TensorFlow\. For more information, see [Enable SageMaker Training Compiler Using the SageMaker Python SDK and Extending SageMaker Framework Deep Learning Containers](training-compiler-enable-tensorflow.md#training-compiler-enable-tensorflow-sdk-extend-container)\.
+ Added support for G5 instances for TensorFlow training jobs\.

**Migration to AWS Deep Learning Containers**

This release passed benchmark testing and is migrated to the following AWS Deep Learning Container:
+ TensorFlow 2\.9\.1

  ```
  763104351884.dkr.ecr.<region>.amazonaws.com/tensorflow-training:2.9.1-gpu-py39-cu112-ubuntu20.04-sagemaker
  ```

  To find a complete list of the pre\-built containers with Amazon SageMaker Training Compiler, see [Supported Frameworks, AWS Regions, Instance Types, and Tested Models](training-compiler-support.md)\.

## SageMaker Training Compiler Release Notes: April 26, 2022<a name="training-compiler-release-notes-20220426"></a>

**Improvements**
+ Added support for all of the AWS Regions where [AWS Deep Learning Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) are in service except the China regions\.

## SageMaker Training Compiler Release Notes: April 12, 2022<a name="training-compiler-release-notes-20220412"></a>

**Currency Updates**
+ Added support for Hugging Face Transformers v4\.17\.0 with TensorFlow v2\.6\.3 and PyTorch v1\.10\.2\.

## SageMaker Training Compiler Release Notes: February 21, 2022<a name="training-compiler-release-notes-20220221"></a>

**Improvements**
+ Completed benchmark test and confirmed training speed\-ups on the `ml.g4dn` instance types\. To find a complete list of tested `ml` instances, see [Supported Instance Types](training-compiler-support.md#training-compiler-supported-instance-types)\.

## SageMaker Training Compiler Release Notes: December 01, 2021<a name="training-compiler-release-notes-20211201"></a>

**New Features**
+ Launched Amazon SageMaker Training Compiler at AWS re:Invent 2021\.

**Migration to AWS Deep Learning Containers**
+ Amazon SageMaker Training Compiler passed benchmark testing and is migrated to AWS Deep Learning Containers\. To find a complete list of the prebuilt containers with Amazon SageMaker Training Compiler, see [Supported Frameworks, AWS Regions, Instance Types, and Tested Models](training-compiler-support.md)\.