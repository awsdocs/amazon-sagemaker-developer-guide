# Supported Frameworks, AWS Regions, Supported Instances, and Tested Models<a name="training-compiler-support"></a>

Before using SageMaker Training Compiler, check if your framework of choice is supported, the instance types are available in your AWS account, and your AWS account is in one of the supported AWS Regions\.

**Note**  
SageMaker Training Compiler is available in the SageMaker Python SDK v2\.70\.0 or later\.

## Supported Frameworks<a name="training-compiler-supported-frameworks"></a>

SageMaker Training Compiler supports the following deep learning frameworks and is available through AWS Deep Learning Containers\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html)

For more information, see [Available Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) in the *AWS Deep Learning Containers GitHub repository*\.

## AWS Regions<a name="training-compiler-availablity-zone"></a>

The [SageMaker Training Compiler Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-training-compiler-containers) are available in the AWS Regions where [AWS Deep Learning Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) are in service except the China regions\.

## Supported Instance Types<a name="training-compiler-supported-instance-types"></a>

SageMaker Training Compiler is tested on and supports the following ML instance types\.
+ P4 instances
+ P3 instances
+ G4dn instances
+ G5 instances \(available for TensorFlow\)

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

### TensorFlow 2\.9\.1<a name="training-compiler-tested-models-tf291"></a>

Tested using [TensorFlow Model Garden](https://github.com/tensorflow/models) with Automatic Mixed Precision \(AMP\)

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html)

\* The batch sizes marked with the asterisk symbol \(\*\) indicate the largest batch size tested by the SageMaker Training Compiler developer team\. For the marked cells, the instance may be able to fit a larger batch size than what is indicated\.

### Transformers 4\.17\.0 with PyTorch 1\.10\.2<a name="training-compiler-tested-models-hf417-pt110"></a>

Tested with `Sequence_Len=512` and Automatic Mixed Precision \(AMP\)

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html)

### Transformers 4\.11\.0 with PyTorch 1\.9\.0<a name="training-compiler-tested-models-hf411-pt190"></a>

Tested with `Sequence_Len=512` and Automatic Mixed Precision \(AMP\)


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

### Transformers 4\.17\.0 with TensorFlow 2\.6\.3<a name="training-compiler-tested-models-hf417-tf263"></a>

Tested with `Sequence_Len=128` and Automatic Mixed Precision \(AMP\)


| Model  | Instance type | Batch size for native frameworks | Batch size for Training Compiler | 
| --- | --- | --- | --- | 
| albert\-base\-v2 | ml\.g4dn\.16xlarge | 136 | 208 | 
| albert\-base\-v2 | ml\.g5\.4xlarge | 219 | 312 | 
| albert\-base\-v2 | ml\.p3\.2xlarge | 152 | 208 | 
| albert\-base\-v2 | ml\.p3\.8xlarge | 152 | 192 | 
| bert\-base\-uncased | ml\.g4dn\.16xlarge | 120 | 101 | 
| bert\-base\-uncased | ml\.g5\.4xlarge | 184 | 160 | 
| bert\-base\-uncased | ml\.p3\.2xlarge | 128 | 108 | 
| bert\-large\-uncased | ml\.g4dn\.16xlarge | 37 | 28 | 
| bert\-large\-uncased | ml\.g5\.4xlarge | 64 | 55 | 
| bert\-large\-uncased | ml\.p3\.2xlarge | 40 | 32 | 
| camembert\-base | ml\.g4dn\.16xlarge | 96 | 100 | 
| camembert\-base | ml\.g5\.4xlarge | 190 | 160 | 
| camembert\-base | ml\.p3\.2xlarge | 129 | 108 | 
| camembert\-base | ml\.p3\.8xlarge | 128 | 104 | 
| distilbert\-base\-uncased | ml\.g4dn\.16xlarge | 210 | 160 | 
| distilbert\-base\-uncased | ml\.g5\.4xlarge | 327 | 288 | 
| distilbert\-base\-uncased | ml\.p3\.2xlarge | 224 | 196 | 
| distilbert\-base\-uncased | ml\.p3\.8xlarge | 192 | 182 | 
| google\_electra\-small\-discriminator | ml\.g4dn\.16xlarge | 336 | 288 | 
| google\_electra\-small\-discriminator | ml\.g5\.4xlarge | 504 | 384 | 
| google\_electra\-small\-discriminator | ml\.p3\.2xlarge | 352 | 323 | 
| gpt2 | ml\.g4dn\.16xlarge | 89 | 64 | 
| gpt2 | ml\.g5\.4xlarge | 140 | 146 | 
| gpt2 | ml\.p3\.2xlarge | 94 | 96 | 
| gpt2 | ml\.p3\.8xlarge | 96 | 88 | 
| jplu\_tf\-xlm\-roberta\-base | ml\.g4dn\.16xlarge | 52 | 16 | 
| jplu\_tf\-xlm\-roberta\-base | ml\.g5\.4xlarge | 64 | 44 | 
| microsoft\_mpnet\-base | ml\.g4dn\.16xlarge | 120 | 100 | 
| microsoft\_mpnet\-base | ml\.g5\.4xlarge | 192 | 160 | 
| microsoft\_mpnet\-base | ml\.p3\.2xlarge | 128 | 104 | 
| microsoft\_mpnet\-base | ml\.p3\.8xlarge | 130 | 92 | 
| roberta\-base | ml\.g4dn\.16xlarge | 108 | 64 | 
| roberta\-base | ml\.g5\.4xlarge | 176 | 142 | 
| roberta\-base | ml\.p3\.2xlarge | 118 | 100 | 
| roberta\-base | ml\.p3\.8xlarge | 112 | 88 | 

### Transformers 4\.11\.0 with TensorFlow 2\.5\.1<a name="training-compiler-tested-models-hf411-tf251"></a>

Tested with `Sequence_Len=128` and Automatic Mixed Precision \(AMP\)


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