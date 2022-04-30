# Supported Frameworks, AWS Regions, Tested Instances, and Tested Models<a name="training-compiler-support"></a>

Before using SageMaker Training Compiler, check if your framework of choice is supported, the instance types are available in your AWS account, and your AWS account is in one of the supported AWS Regions\.

**Note**  
SageMaker Training Compiler is available in the SageMaker Python SDK v2\.70\.0 or later\.

## Supported Frameworks<a name="training-compiler-supported-frameworks"></a>

SageMaker Training Compiler supports the following deep learning frameworks and is available through AWS Deep Learning Containers\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html)

For more information, see [SageMaker Training Compiler containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-training-compiler-containers) in the *AWS Deep Learning Containers GitHub repository*\.

## AWS Regions<a name="training-compiler-availablity-zone"></a>

The [SageMaker Training Compiler Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-training-compiler-containers) are available in the AWS Regions where [AWS Deep Learning Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) are in service except the China regions\.

## Tested Instance Types<a name="training-compiler-supported-instance-types"></a>

SageMaker Training Compiler is tested on but not limited to the following ML instance types\.


| Instance type | 
| --- | 
| ml\.p3\.2xlarge  | 
| ml\.p3\.8xlarge  | 
| ml\.p3\.16xlarge  | 
| ml\.p3dn\.24xlarge  | 
| ml\.p4d\.24xlarge | 
| ml\.g4dn\.xlarge | 
| ml\.g4dn\.2xlarge | 
| ml\.g4dn\.4xlarge | 
| ml\.g4dn\.8xlarge | 
| ml\.g4dn\.12xlarge | 
| ml\.g4dn\.16xlarge | 

For specs of the instance types, see the **Accelerated Computing** section in the [Amazon EC2 Instance Types page](http://aws.amazon.com/ec2/instance-types/)\. For information about instance pricing, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

If you encountered an error message similar to the following, follow the instructions at [Request a service quota increase for SageMaker resources](https://docs.aws.amazon.com/sagemaker/latest/dg/regions-quotas.html#service-limit-increase-request-procedure)\.

```
ResourceLimitExceeded: An error occurred (ResourceLimitExceeded) when calling
the CreateTrainingJob operation: The account-level service limit 'ml.p3dn.24xlarge
for training job usage' is 0 Instances, with current utilization of 0 Instances
and a request delta of 1 Instances.
Please contact AWS support to request an increase for this limit.
```

## Tested Models<a name="training-compiler-tested-models"></a>

The following table includes a list of the models that have been tested with SageMaker Training Compiler\. For reference, the largest batch size that is able to fit into memory is also included alongside other training parameters\. SageMaker Training Compiler can change the memory footprint of the model training process; as a result, a larger batch size can often be used during the training process, further decreasing total training time\. You must retune your model with the compiler enabled to find this increased batch size\. To save time, use the reference table to pick up the largest batch sizes that can be used with the tested models\.

**Note**  
The batch sizes are local batch size that fit into each individual GPU of an instance type\. You should also adjust the learning rate when changing the batch size\.


**TensorFlow \(Sequence\_Len=128, Automatic Mixed Precision \(AMP\)\)**  

| Single\-node single\-GPU | Model  | Instance type | Batch size for native | Batch size for Training Compiler | 
| --- | --- | --- | --- | --- | 
| albert\-base\-v2  | ml\.p3\.2xlarge | 128 | 128 | 
| bart\-base  | ml\.p3\.2xlarge | 12 | 64 | 
| bart\-large  | ml\.p3\.2xlarge | 4 | 28 | 
| bert\-base\-cased  | ml\.p3\.2xlarge | 16 | 128 | 
| bert\-base\-chinese | ml\.p3\.2xlarge | 16 | 128 | 
| bert\-base\-multilingual\-cased  | ml\.p3\.2xlarge | 12 | 64 | 
| bert\-base\-multilingual\-uncased  | ml\.p3\.2xlarge | 16 | 96 | 
| bert\-base\-uncased | ml\.p3\.2xlarge | 16 | 96 | 
| bert\-large\-uncased  | ml\.p3\.2xlarge | 4 | 24 | 
| cl\-tohoku/bert\-base\-japanese  | ml\.p3\.2xlarge | 16 | 128 | 
| cl\-tohoku/bert\-base\-japanese\-whole\-word\-masking  | ml\.p3\.2xlarge | 16 | 128 | 
| distilbert\-base\-sst2  | ml\.p3\.2xlarge | 32 | 128 | 
| distilbert\-base\-uncased  | ml\.p3\.2xlarge | 32 | 128 | 
| distilgpt2 | ml\.p3\.2xlarge | 32 | 128 | 
| gpt2  | ml\.p3\.2xlarge | 12 | 64 | 
| gpt2\-large  | ml\.p3\.2xlarge | 2 | 24 | 
| jplu/tf\-xlm\-roberta\-base  | ml\.p3\.2xlarge | 12 | 32 | 
| roberta\-base  | ml\.p3\.2xlarge | 4 | 64 | 
| roberta\-large  | ml\.p3\.2xlarge | 4 | 64 | 
| t5\-base  | ml\.p3\.2xlarge | 64 | 64 | 
| t5\-small  | ml\.p3\.2xlarge | 128 | 128 | 


**PyTorch \(Sequence\_Len=512, Automatic Mixed Precision \(AMP\)\)**  

| Single\-node single\-GPU | Model  | Instance type | Batch size for native | Batch size for Training Compiler | 
| --- | --- | --- | --- | --- | 
| albert\-base\-v2  | ml\.p3\.2xlarge | 12 | 32 | 
| bert\-base\-cased  | ml\.p3\.2xlarge | 14 | 24 | 
| bert\-base\-chinese | ml\.p3\.2xlarge | 16 | 24 | 
| bert\-base\-multilingual\-cased  | ml\.p3\.2xlarge | 4 | 16 | 
| bert\-base\-multilingual\-uncased  | ml\.p3\.2xlarge | 8 | 16 | 
| bert\-base\-uncased  | ml\.p3\.2xlarge | 12 | 24 | 
| cl\-tohoku/bert\-base\-japanese\-whole\-word\-masking | ml\.p3\.2xlarge | 12 | 24 | 
| cl\-tohoku/bert\-base\-japanese  | ml\.p3\.2xlarge | 12 | 24 | 
| distilbert\-base\-uncased  | ml\.p3\.2xlarge | 28 | 32 | 
| distilbert\-base\-uncased\-finetuned\-sst\-2\-english | ml\.p3\.2xlarge | 28 | 32 | 
| distilgpt2  | ml\.p3\.2xlarge | 16 | 32 | 
| facebook/bart\-base  | ml\.p3\.2xlarge | 4 | 8 | 
| gpt2 | ml\.p3\.2xlarge | 6 | 20 | 
| nreimers/MiniLMv2\-L6\-H384\-distilled\-from\-RoBERTa\-Large  | ml\.p3\.2xlarge | 20 | 32 | 
| roberta\-base  | ml\.p3\.2xlarge | 12 | 20 | 


**PyTorch \(Sequence\_Len=512, Automatic Mixed Precision \(AMP\)\)**  

| Single\-node multi\-GPU | Model  | Instance type | Batch size for native | Batch size for Training Compiler | 
| --- | --- | --- | --- | --- | 
| bert\-base\-chinese  | ml\.p3\.8xlarge | 16 | 26 | 
| bert\-base\-multilingual\-cased  | ml\.p3\.8xlarge | 6 | 16 | 
| bert\-base\-multilingual\-uncased | ml\.p3\.8xlarge | 6 | 16 | 
| bert\-base\-uncased  | ml\.p3\.8xlarge | 14 | 24 | 
| distilbert\-base\-uncased  | ml\.p3\.8xlarge | 14 | 32 | 
| distilgpt2 | ml\.p3\.8xlarge | 6 | 32 | 
| facebook/bart\-base | ml\.p3\.8xlarge | 8 | 16 | 
| gpt2  | ml\.p3\.8xlarge | 8 | 20 | 
| roberta\-base  | ml\.p3\.8xlarge | 12 | 20 | 