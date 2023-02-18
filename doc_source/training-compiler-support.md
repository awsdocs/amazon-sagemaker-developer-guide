# Supported Frameworks, AWS Regions, Instance Types, and Tested Models<a name="training-compiler-support"></a>

Before using SageMaker Training Compiler, check if your framework of choice is supported, the instance types are available in your AWS account, and your AWS account is in one of the supported AWS Regions\.

**Note**  
SageMaker Training Compiler is available in the SageMaker Python SDK v2\.70\.0 or later\.

## Supported Frameworks<a name="training-compiler-supported-frameworks"></a>

SageMaker Training Compiler supports the following deep learning frameworks and is available through AWS Deep Learning Containers\.

**Topics**
+ [PyTorch](#training-compiler-supported-frameworks-pytorch)
+ [TensorFlow](#training-compiler-supported-frameworks-tensorflow)

### PyTorch<a name="training-compiler-supported-frameworks-pytorch"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html)

### TensorFlow<a name="training-compiler-supported-frameworks-tensorflow"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html)

For more information, see [Available Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) in the *AWS Deep Learning Containers GitHub repository*\.

## AWS Regions<a name="training-compiler-availablity-zone"></a>

The [SageMaker Training Compiler Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-training-compiler-containers) are available in the AWS Regions where [AWS Deep Learning Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) are in service except the China regions\.

## Supported Instance Types<a name="training-compiler-supported-instance-types"></a>

SageMaker Training Compiler is tested on and supports the following ML instance types\.
+ P4 instances
+ P3 instances
+ G4dn instances
+ G5 instances

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

The following table includes a list of the models that have been tested with SageMaker Training Compiler\. For reference, the largest batch size that is able to fit into memory is also included alongside other training parameters\. SageMaker Training Compiler can change the memory footprint of the model training process; as a result, a larger batch size can often be used during the training process, further decreasing total training time\. In some cases, SageMaker Training Compiler intelligently promotes caching which leads to a decrease in the largest batch size that can fit on the GPU\. You must retune your model hyperparameters and find an optimal batch size for your case\. To save time, use the following reference tables to look up a batch size that can be a good starting point for your use case\.

**Note**  
The batch sizes are local batch size that fit into each individual GPU in the respective instance type\. You should also adjust the learning rate when changing the batch size\.

### PyTorch 1\.13\.1<a name="training-compiler-tested-models-pt1131"></a>

**Natural language processing \(NLP\) models**

The following models are tested for training jobs for all combinations of single\-node and multi\-node with single or multi GPU cores and Automatic Mixed Precision \(AMP\) as indicated\.


| Single\-node/multi\-node single\-GPU/multi\-GPU | Model | Dataset | Instance type | Precision | Sequence Length | Batch size for native frameworks  | Batch size for SageMaker Training Compiler  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 128 | 80 | 192 | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 128 | 332 | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 128 | 80 | 224 | 
| bert\-base\-uncased | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 160 | 288 | 
| camembert\-base | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 160 | 280 | 
| distilbert\-base\-uncased | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 240 | 472 | 
| distilgpt2 | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 128 | 77 | 128 | 
| distilgpt2 | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 138 | 390 | 
| distilgpt2 | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 128 | 96 | 256 | 
| distilroberta\-base | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 128 | 96 | 192 | 
| distilroberta\-base | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 171 | 380 | 
| distilroberta\-base | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 128 | 112 | 256 | 
| gpt2 | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 128 | 52 | 152 | 
| gpt2 | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 84 | 240 | 
| gpt2 | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 128 | 58 | 164 | 
| microsoft/deberta\-base | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 128 | 48 | 128 | 
| microsoft/deberta\-base | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 84 | 207 | 
| microsoft/deberta\-base | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 128 | 53 | 133 | 
| roberta\-base | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 125 | 224 | 
| facebook/bart\-base | xsum | g4dn\.16xlarge | float16 | 128 | 10 | 16 | 
| facebook/bart\-base | xsum | g5\.4xlarge | float16 | 128 | 16 | 32 | 
| facebook/bart\-large | xsum | g5\.4xlarge | float16 | 128 | 5 | 8 | 
| facebook/bart\-large | xsum | p3\.2xlarge | float16 | 128 | 2 | 4 | 
| xlm\-roberta\-base | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 128 | 16 | 31 | 
| xlm\-roberta\-base | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 128 | 18 | 50 | 
| xlnet\-base\-cased | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 128 | 240 | 
| bert\-base\-uncased | wikitext\-103\-v1 | g5\.48xlarge | float16 | 512 | 29 | 50 | 
| distilbert\-base\-uncased | wikitext\-103\-v1 | g5\.48xlarge | float16 | 512 | 45 | 64 | 
| gpt2 | wikitext\-103\-v1 | g5\.48xlarge | float16 | 512 | 18 | 45 | 
| roberta\-base | wikitext\-103\-v1 | g5\.48xlarge | float16 | 512 | 23 | 44 | 
| gpt2 | wikitext\-103\-v1 | p4d\.24xlarge | float16 | 512 | 36 | 64 | 

**Computer Vision \(CV\) models**

Tested using [TensorFlow Model Garden](https://github.com/tensorflow/models) with Automatic Mixed Precision \(AMP\) as indicated\.


| Single/multi\-node single/multi\-GPU | Model | Dataset | Instance type | Precision | Batch size for native frameworks  | Batch size for SageMaker Training Compiler  | 
| --- | --- | --- | --- | --- | --- | --- | 
| ResNet152 | food101 | g4dn\.16xlarge | float16 | 128 | 144 | 
| ResNet152 | food101 | g5\.4xlarge | float16 | 128 | 192 | 
| ResNet152 | food101 | p3\.2xlarge | float16 | 152 | 156 | 
| ViT | food101 | g4dn\.16xlarge | float16 | 512 | 512 | 
| ViT | food101 | g5\.4xlarge | float16 | 992 | 768 | 
| ViT | food101 | p3\.2xlarge | float16 | 848 | 768 | 

### PyTorch 1\.12\.0<a name="training-compiler-tested-models-pt1120"></a>

**Natural language processing \(NLP\) models**

The following models are tested for training jobs for all combinations of single\-node and multi\-node with single or multi GPU cores and Automatic Mixed Precision \(AMP\) as indicated\.


| Single\-node/multi\-node single\-GPU/multi\-GPU | Model | Dataset | Instance type | Precision | Sequence Length | Batch size for native frameworks  | Batch size for SageMaker Training Compiler  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 128 | 248 | 
| bert\-base\-uncased | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 160 | 288 | 
| camembert\-base | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 160 | 279 | 
| camembert\-base | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 128 | 105 | 164 | 
| distilgpt2 | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 136 | 256 | 
| distilgpt2 | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 128 | 80 | 118 | 
| gpt2 | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 84 | 240 | 
| gpt2 | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 128 | 80 | 119 | 
| microsoft/deberta\-base | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 93 | 197 | 
| microsoft/deberta\-base | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 128 | 113 | 130 | 
| roberta\-base | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 125 | 224 | 
| roberta\-base | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 128 | 78 | 112 | 
| xlnet\-base\-cased | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 138 | 240 | 
| bert\-base\-uncased | wikitext\-103\-v1 | ml\.p4d\.24xlarge | float16 | 512 |  | 52 | 
| distilbert\-base\-uncased | wikitext\-103\-v1 | ml\.p4d\.24xlarge | float16 | 512 |  | 160 | 
| gpt2 | wikitext\-103\-v1 | ml\.p4d\.24xlarge | float16 | 512 |  | 25 | 
| roberta\-base | wikitext\-103\-v1 | ml\.p4d\.24xlarge | float16 | 512 |  | 64 | 

### TensorFlow 2\.11\.0<a name="training-compiler-tested-models-tf2110"></a>

**Computer Vision \(CV\) models**

Tested using [TensorFlow Model Garden](https://github.com/tensorflow/models) with Automatic Mixed Precision \(AMP\) as indicated\.


| Single/multi\-node single/multi\-GPU | Model | Dataset | Instance type | Precision | Batch size for native frameworks  | Batch size for SageMaker Training Compiler  | 
| --- | --- | --- | --- | --- | --- | --- | 
| MaskRCNN\-ResNet50\-FPN | COCO\-2017 | ml\.g5\.2xlarge | float16 | 6 | 8 | 
| MaskRCNN\-ResNet50\-FPN | COCO\-2017 | ml\.p3\.2xlarge | float16 | 4 | 6 | 
| ResNet50 | ImageNet | ml\.g5\.2xlarge | float16 | 192 | 256 | 
| ResNet50 | ImageNet | ml\.p3\.2xlarge | float16 | 256 | 256 | 
| ResNet101 | ImageNet | ml\.g5\.2xlarge | float16 | 128 | 256 | 
| ResNet101 | ImageNet | ml\.p3\.2xlarge | float16 | 128 | 128 | 
| ResNet152 | ImageNet | ml\.g5\.2xlarge | float16 | 128 | 224 | 
| ResNet152 | ImageNet | ml\.p3\.2xlarge | float16 | 128 | 128 | 
| VisionTransformer | ImageNet | ml\.g5\.2xlarge | float16 | 112 | 144 | 
| VisionTransformer | ImageNet | ml\.p3\.2xlarge | float16 | 96 | 128 | 

**Natural Language Processing \(NLP\) models**

Tested using [Transformer models](https://github.com/huggingface/transformers) with `Sequence_Len=128` and Automatic Mixed Precision \(AMP\) as indicated\.


| Single/multi\-node single/multi\-GPU | Model | Dataset | Instance type | Precision | Batch size for native frameworks  | Batch size for SageMaker Training Compiler  | 
| --- | --- | --- | --- | --- | --- | --- | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 160 | 197 | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 95 | 127 | 
| bert\-base\-uncased | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 160 | 128 | 
| bert\-base\-uncased | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 104 | 111 | 
| bert\-large\-uncased | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 65 | 48 | 
| bert\-large\-uncased | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 40 | 35 | 
| camembert\-base | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 162 | 
| camembert\-base | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 105 | 111 | 
| distilbert\-base\-uncased | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 256 | 264 | 
| distilbert\-base\-uncased | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 128 | 169 | 
| gpt2 | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 120 | 
| gpt2 | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 80 | 83 | 
| jplu/tf\-xlm\-roberta\-base | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 32 | 32 | 
| jplu/tf\-xlm\-roberta\-base | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 32 | 36 | 
| microsoft/mpnet\-base | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 144 | 160 | 
| microsoft/mpnet\-base | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 106 | 110 | 
| roberta\-base | wikitext\-2\-raw\-v1 | ml\.g5\.2xlarge | float16 | 128 | 128 | 
| roberta\-base | wikitext\-2\-raw\-v1 | ml\.p3\.2xlarge | float16 | 72 | 98 | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | ml\.g5\.48xlarge | float16 | 128 | 192 | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | ml\.p3\.16xlarge | float16 | 95 | 96 | 
| distilbert\-base\-uncased | wikitext\-2\-raw\-v1 | ml\.g5\.48xlarge | float16 | 256 | 256 | 
| distilbert\-base\-uncased | wikitext\-2\-raw\-v1 | ml\.p3\.16xlarge | float16 | 140 | 184 | 
| google/electra\-small\-discriminator | wikitext\-2\-raw\-v1 | ml\.g5\.48xlarge | float16 | 256 | 384 | 
| google/electra\-small\-discriminator | wikitext\-2\-raw\-v1 | ml\.p3\.16xlarge | float16 | 256 | 268 | 
| gpt2 | wikitext\-2\-raw\-v1 | ml\.g5\.48xlarge | float16 | 116 | 116 | 
| gpt2 | wikitext\-2\-raw\-v1 | ml\.p3\.16xlarge | float16 | 85 | 83 | 
| gpt2 | wikitext\-2\-raw\-v1 | ml\.p4d\.24xlarge | float16 | 94 | 110 | 
| microsoft/mpnet\-base | wikitext\-2\-raw\-v1 | ml\.g5\.48xlarge | float16 | 187 | 164 | 
| microsoft/mpnet\-base | wikitext\-2\-raw\-v1 | ml\.p3\.16xlarge | float16 | 106 | 111 | 

### TensorFlow 2\.10\.0<a name="training-compiler-tested-models-tf2100"></a>

**Computer Vision \(CV\) models**

Tested using [TensorFlow Model Garden](https://github.com/tensorflow/models) with Automatic Mixed Precision \(AMP\) as indicated\.


| Single\-node single\-GPU/multi\-GPU | Model | Dataset | Instance type | Precision | Batch size for native frameworks  | Batch size for SageMaker Training Compiler  | 
| --- | --- | --- | --- | --- | --- | --- | 
| DetectionTransformer\-ResNet50 | COCO\-2017 | ml\.g4dn\.2xlarge | float32 | 2 | 4 | 
| DetectionTransformer\-ResNet50 | COCO\-2017 | ml\.g5\.2xlarge | float32 | 3 | 6 | 
| DetectionTransformer\-ResNet50 | COCO\-2017 | ml\.p3\.2xlarge | float32 | 2 | 4 | 
| MaskRCNN\-ResNet50\-FPN | COCO\-2017 | ml\.g4dn\.2xlarge | float16 | 4 | 6 | 
| MaskRCNN\-ResNet50\-FPN | COCO\-2017 | ml\.g5\.2xlarge | float16 | 6 | 8 | 
| MaskRCNN\-ResNet50\-FPN | COCO\-2017 | ml\.g5\.48xlarge | float16 | 48 | 64 | 
| MaskRCNN\-ResNet50\-FPN | COCO\-2017 | ml\.p3\.2xlarge | float16 | 4 | 6 | 
| ResNet50 | ImageNet | ml\.g4dn\.2xlarge | float16 | 224 | 256 | 
| ResNet50 | ImageNet | ml\.g5\.2xlarge | float16 | 192 | 160 | 
| ResNet50 | ImageNet | ml\.g5\.48xlarge | float16 | 2048 | 2048 | 
| ResNet50 | ImageNet | ml\.p3\.2xlarge | float16 | 224 | 160 | 
| ResNet101 | ImageNet | ml\.g4dn\.2xlarge | float16 | 160 | 128 | 
| ResNet101 | ImageNet | ml\.g5\.2xlarge | float16 | 192 | 256 | 
| ResNet101 | ImageNet | ml\.g5\.48xlarge | float16 | 2048 | 2048 | 
| ResNet101 | ImageNet | ml\.p3\.2xlarge | float16 | 160 | 224 | 
| ResNet152 | ImageNet | ml\.g4dn\.2xlarge | float16 | 128 | 128 | 
| ResNet152 | ImageNet | ml\.g5\.2xlarge | float16 | 192 | 224 | 
| ResNet152 | ImageNet | ml\.g5\.48xlarge | float16 | 1536 | 1792 | 
| ResNet152 | ImageNet | ml\.p3\.2xlarge | float16 | 128 | 160 | 
| VisionTransformer | ImageNet | ml\.g4dn\.2xlarge | float16 | 80 | 128 | 
| VisionTransformer | ImageNet | ml\.g5\.2xlarge | float16 | 112 | 144 | 
| VisionTransformer | ImageNet | ml\.g5\.48xlarge | float16 | 896 | 1152 | 
| VisionTransformer | ImageNet | ml\.p3\.2xlarge | float16 | 80 | 128 | 

**Natural Language Processing \(NLP\) models**

Tested using [Transformer models](https://github.com/huggingface/transformers) with `Sequence_Len=128` and Automatic Mixed Precision \(AMP\) as indicated\.


| Single\-node single\-GPU/multi\-GPU | Model | Dataset | Instance type | Precision | Batch size for native frameworks  | Batch size for SageMaker Training Compiler  | 
| --- | --- | --- | --- | --- | --- | --- | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 128 | 112 | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 128 | 128 | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | p3\.8xlarge | float16 | 128 | 135 | 
| albert\-base\-v2 | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 191 | 
| bert\-base\-uncased | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 64 | 94 | 
| bert\-base\-uncased | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 96 | 101 | 
| bert\-base\-uncased | wikitext\-2\-raw\-v1 | p3\.8xlarge | float16 | 96 | 96 | 
| bert\-base\-uncased | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 128 | 
| bert\-large\-uncased | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 35 | 21 | 
| bert\-large\-uncased | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 39 | 26 | 
| bert\-large\-uncased | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 60 | 50 | 
| camembert\-base | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 96 | 90 | 
| camembert\-base | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 96 | 98 | 
| camembert\-base | wikitext\-2\-raw\-v1 | p3\.8xlarge | float16 | 96 | 96 | 
| camembert\-base | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 128 | 
| distilbert\-base\-uncased | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 256 | 160 | 
| distilbert\-base\-uncased | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 128 | 176 | 
| distilbert\-base\-uncased | wikitext\-2\-raw\-v1 | p3\.8xlarge | float16 | 128 | 160 | 
| distilbert\-base\-uncased | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 256 | 258 | 
| google\_electra\-small\-discriminator | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 256 | 216 | 
| google\_electra\-small\-discriminator | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 256 | 230 | 
| google\_electra\-small\-discriminator | wikitext\-2\-raw\-v1 | p3\.8xlarge | float16 | 256 | 224 | 
| google\_electra\-small\-discriminator | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 256 | 320 | 
| gpt2 | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 80 | 64 | 
| gpt2 | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 80 | 77 | 
| gpt2 | wikitext\-2\-raw\-v1 | p3\.8xlarge | float16 | 80 | 72 | 
| gpt2 | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 120 | 
| jplu\_tf\-xlm\-roberta\-base | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 28 | 24 | 
| jplu\_tf\-xlm\-roberta\-base | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 32 | 24 | 
| jplu\_tf\-xlm\-roberta\-base | wikitext\-2\-raw\-v1 | p3\.8xlarge | float16 | 32 | 26 | 
| jplu\_tf\-xlm\-roberta\-base | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 66 | 52 | 
| microsoft\_mpnet\-base | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 96 | 92 | 
| microsoft\_mpnet\-base | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 96 | 101 | 
| microsoft\_mpnet\-base | wikitext\-2\-raw\-v1 | p3\.8xlarge | float16 | 96 | 101 | 
| microsoft\_mpnet\-base | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 152 | 
| roberta\-base | wikitext\-2\-raw\-v1 | g4dn\.16xlarge | float16 | 64 | 72 | 
| roberta\-base | wikitext\-2\-raw\-v1 | p3\.2xlarge | float16 | 64 | 84 | 
| roberta\-base | wikitext\-2\-raw\-v1 | p3\.8xlarge | float16 | 64 | 86 | 
| roberta\-base | wikitext\-2\-raw\-v1 | g5\.4xlarge | float16 | 128 | 128 | 

### TensorFlow 2\.9\.1<a name="training-compiler-tested-models-tf291"></a>

Tested using [TensorFlow Model Garden](https://github.com/tensorflow/models) with Automatic Mixed Precision \(AMP\)\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html)

\* The batch sizes marked with the asterisk symbol \(\*\) indicate the largest batch size tested by the SageMaker Training Compiler developer team\. For the marked cells, the instance may be able to fit a larger batch size than what is indicated\.

### Transformers 4\.21\.1 with PyTorch 1\.11\.0<a name="training-compiler-tested-models-hf421-pt111"></a>

Tested with `Sequence_Len=512` and Automatic Mixed Precision \(AMP\)\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html)

### Transformers 4\.17\.0 with PyTorch 1\.10\.2<a name="training-compiler-tested-models-hf417-pt110"></a>

Tested with `Sequence_Len=512` and Automatic Mixed Precision \(AMP\)\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html)

### Transformers 4\.11\.0 with PyTorch 1\.9\.0<a name="training-compiler-tested-models-hf411-pt190"></a>

Tested with `Sequence_Len=512` and Automatic Mixed Precision \(AMP\)\.


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

Tested with `Sequence_Len=128` and Automatic Mixed Precision \(AMP\)\.


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

Tested with `Sequence_Len=128` and Automatic Mixed Precision \(AMP\)\.


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