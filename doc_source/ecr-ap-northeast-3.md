# Docker Registry Paths and Example Code for Asia Pacific \(Osaka\) \(ap\-northeast\-3\)<a name="ecr-ap-northeast-3"></a>

The following topics list parameters for each of the algorithms and deep learning containers in this region provided by Amazon SageMaker\.

**Topics**
+ [AutoGluon \(algorithm\)](#autogluon-ap-northeast-3.title)
+ [BlazingText \(algorithm\)](#blazingtext-ap-northeast-3.title)
+ [Clarify \(algorithm\)](#clarify-ap-northeast-3.title)
+ [Debugger \(algorithm\)](#debugger-ap-northeast-3.title)
+ [Factorization Machines \(algorithm\)](#factorization-machines-ap-northeast-3.title)
+ [Hugging Face \(algorithm\)](#huggingface-ap-northeast-3.title)
+ [IP Insights \(algorithm\)](#ipinsights-ap-northeast-3.title)
+ [Image classification \(algorithm\)](#image-classification-ap-northeast-3.title)
+ [Inferentia MXNet \(DLC\)](#inferentia-mxnet-ap-northeast-3.title)
+ [Inferentia PyTorch \(DLC\)](#inferentia-pytorch-ap-northeast-3.title)
+ [K\-Means \(algorithm\)](#kmeans-ap-northeast-3.title)
+ [KNN \(algorithm\)](#knn-ap-northeast-3.title)
+ [Linear Learner \(algorithm\)](#linear-learner-ap-northeast-3.title)
+ [MXNet \(DLC\)](#mxnet-ap-northeast-3.title)
+ [Model Monitor \(algorithm\)](#model-monitor-ap-northeast-3.title)
+ [NTM \(algorithm\)](#ntm-ap-northeast-3.title)
+ [Neo Image Classification \(algorithm\)](#image-classification-neo-ap-northeast-3.title)
+ [Neo MXNet \(DLC\)](#neo-mxnet-ap-northeast-3.title)
+ [Neo PyTorch \(DLC\)](#neo-pytorch-ap-northeast-3.title)
+ [Neo Tensorflow \(DLC\)](#neo-tensorflow-ap-northeast-3.title)
+ [Neo XGBoost \(algorithm\)](#xgboost-neo-ap-northeast-3.title)
+ [Object Detection \(algorithm\)](#object-detection-ap-northeast-3.title)
+ [Object2Vec \(algorithm\)](#object2vec-ap-northeast-3.title)
+ [PCA \(algorithm\)](#pca-ap-northeast-3.title)
+ [PyTorch \(DLC\)](#pytorch-ap-northeast-3.title)
+ [Random Cut Forest \(algorithm\)](#randomcutforest-ap-northeast-3.title)
+ [Scikit\-learn \(algorithm\)](#sklearn-ap-northeast-3.title)
+ [Semantic Segmentation \(algorithm\)](#semantic-segmentation-ap-northeast-3.title)
+ [Seq2Seq \(algorithm\)](#seq2seq-ap-northeast-3.title)
+ [Tensorflow \(DLC\)](#tensorflow-ap-northeast-3.title)
+ [Tensorflow Inferentia \(DLC\)](#inferentia-tensorflow-ap-northeast-3.title)
+ [XGBoost \(algorithm\)](#xgboost-ap-northeast-3.title)

## AutoGluon \(algorithm\)<a name="autogluon-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='autogluon',region='ap-northeast-3',image_scope='inference')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/autogluon\-training:<tag> | 0\.3\.1 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/autogluon\-inference:<tag> | 0\.3\.1 | inference | 

## BlazingText \(algorithm\)<a name="blazingtext-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='blazingtext',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/blazingtext:<tag> | 1 | inference, training | 

## Clarify \(algorithm\)<a name="clarify-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='clarify',region='ap-northeast-3',version='1.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 912233562940\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-clarify\-processing:<tag> | 1\.0 | processing | 

## Debugger \(algorithm\)<a name="debugger-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='debugger',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 479947661362\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-debugger\-rules:<tag> | latest | debugger | 

## Factorization Machines \(algorithm\)<a name="factorization-machines-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='factorization-machines',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/factorization\-machines:<tag> | 1 | inference, training | 

## Hugging Face \(algorithm\)<a name="huggingface-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='huggingface',region='ap-northeast-3',version='4.4.2',image_scope='training',base_framework_version='tensorflow2.4.1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.11\.0 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.11\.0 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.11\.0 | inference | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.11\.0 | inference | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.6\.1 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.6\.1 | inference | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.6\.1 | inference | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.5\.0 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.5\.0 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.4\.2 | training | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.4\.2 | training | 

## IP Insights \(algorithm\)<a name="ipinsights-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ipinsights',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/ipinsights:<tag> | 1 | inference, training | 

## Image classification \(algorithm\)<a name="image-classification-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/image\-classification:<tag> | 1 | inference, training | 

## Inferentia MXNet \(DLC\)<a name="inferentia-mxnet-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-mxnet',region='ap-northeast-3',version='1.5.1',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-neo\-mxnet:<tag> | 1\.5\.1 | inference | inf | py3 | 

## Inferentia PyTorch \(DLC\)<a name="inferentia-pytorch-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-pytorch',region='ap-northeast-3',version='1.5.1',py_version='py3')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-neo\-pytorch:<tag> | 1\.5\.1 | inference | inf | py3 | 

## K\-Means \(algorithm\)<a name="kmeans-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='kmeans',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/kmeans:<tag> | 1 | inference, training | 

## KNN \(algorithm\)<a name="knn-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='knn',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/knn:<tag> | 1 | inference, training | 

## Linear Learner \(algorithm\)<a name="linear-learner-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='linear-learner',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/linear\-learner:<tag> | 1 | inference, training | 

## MXNet \(DLC\)<a name="mxnet-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='mxnet',region='ap-northeast-3',version='1.4.1',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-training:<tag> | 1\.7\.0 | training | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-inference:<tag> | 1\.7\.0 | inference | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.7\.0 | eia | CPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.5\.1 | eia | CPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-training:<tag> | 1\.4\.1 | training | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-inference:<tag> | 1\.4\.1 | inference | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.4\.1 | eia | CPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-mxnet\-serving\-eia:<tag> | 1\.4\.0 | eia | CPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-mxnet\-eia:<tag> | 1\.3\.0 | eia | CPU | py2, py3 | 

## Model Monitor \(algorithm\)<a name="model-monitor-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='model-monitor',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 990339680094\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-model\-monitor\-analyzer:<tag> |  | monitoring | 

## NTM \(algorithm\)<a name="ntm-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ntm',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/ntm:<tag> | 1 | inference, training | 

## Neo Image Classification \(algorithm\)<a name="image-classification-neo-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification-neo',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/image\-classification\-neo:<tag> | latest | inference | 

## Neo MXNet \(DLC\)<a name="neo-mxnet-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-mxnet',region='ap-northeast-3',version='1.8',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-inference\-mxnet:<tag> | 1\.8 | inference | CPU, GPU | py3 | 

## Neo PyTorch \(DLC\)<a name="neo-pytorch-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-pytorch',region='ap-northeast-3',version='1.6',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.6 | inference | CPU, GPU | py3 | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.5 | inference | CPU, GPU | py3 | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.4 | inference | CPU, GPU | py3 | 

## Neo Tensorflow \(DLC\)<a name="neo-tensorflow-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-tensorflow',region='ap-northeast-3',version='1.15.3',py_version='py3',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-inference\-tensorflow:<tag> | 1\.15\.3 | inference | CPU, GPU | py3 | 

## Neo XGBoost \(algorithm\)<a name="xgboost-neo-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost-neo',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/xgboost\-neo:<tag> | latest | inference | 

## Object Detection \(algorithm\)<a name="object-detection-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object-detection',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/object\-detection:<tag> | 1 | inference, training | 

## Object2Vec \(algorithm\)<a name="object2vec-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object2vec',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/object2vec:<tag> | 1 | inference, training | 

## PCA \(algorithm\)<a name="pca-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pca',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pca:<tag> | 1 | inference, training | 

## PyTorch \(DLC\)<a name="pytorch-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pytorch',region='ap-northeast-3',version='1.8.0',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.9\.0 | inference | CPU, GPU | py38 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.9\.0 | training | CPU, GPU | py38 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.1 | inference | CPU, GPU | py3, py36 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.1 | training | CPU, GPU | py3, py36 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py3, py36 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py3, py36 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.7\.1 | inference | CPU, GPU | py3, py36 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.7\.1 | training | CPU, GPU | py3, py36 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py3, py36 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py3, py36 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.5\.0 | inference | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.5\.0 | training | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.4\.0 | inference | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference\-eia:<tag> | 1\.3\.1 | eia | CPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.3\.1 | inference | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.3\.1 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.2\.0 | inference | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.2\.0 | training | CPU, GPU | py2, py3 | 

## Random Cut Forest \(algorithm\)<a name="randomcutforest-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='randomcutforest',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/randomcutforest:<tag> | 1 | inference, training | 

## Scikit\-learn \(algorithm\)<a name="sklearn-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sklearn',region='ap-northeast-3',version='0.23-1',image_scope='inference')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.23\-1 | inference, training | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.20\.0 | inference, training | 

## Semantic Segmentation \(algorithm\)<a name="semantic-segmentation-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='semantic-segmentation',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/semantic\-segmentation:<tag> | 1 | inference, training | 

## Seq2Seq \(algorithm\)<a name="seq2seq-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='seq2seq',region='ap-northeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/seq2seq:<tag> | 1 | inference, training | 

## Tensorflow \(DLC\)<a name="tensorflow-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='tensorflow',region='ap-northeast-3',version='1.12.0',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.5\.1 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.5\.1 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.5\.0 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.3 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.3 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.1 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.1 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.2 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.2 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.1 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.1 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.3\.0 | eia | CPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.0 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.0 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.2 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.2 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.1 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.1 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.0 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.0 | training | CPU, GPU | py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.3 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.3 | training | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.2 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.2 | training | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.1 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.1 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.0 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.0 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.4 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.4 | training | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.3 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.3 | training | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.2 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.2 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.1 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.1 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.0\.0 | eia | CPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.0 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.0 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.5 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.5 | training | CPU, GPU | py3, py36, py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.4 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.4 | training | CPU, GPU | py3, py36, py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.3 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.3 | training | CPU, GPU | py2, py3, py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.2 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.2 | training | CPU, GPU | py2, py3, py37 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.15\.0 | eia | CPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.0 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.0 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.14\.0 | eia | CPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.14\.0 | inference | CPU, GPU | \- | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.14\.0 | training | CPU, GPU | py2, py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.13\.1 | training | CPU, GPU | py3 | 
| 364406365360\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.13\.0 | inference | CPU, GPU | \- | 

## Tensorflow Inferentia \(DLC\)<a name="inferentia-tensorflow-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-tensorflow',region='ap-northeast-3',version='1.15.0',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 925152966179\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-neo\-tensorflow:<tag> | 1\.15\.0 | inference | inf | py3 | 

## XGBoost \(algorithm\)<a name="xgboost-ap-northeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost',region='ap-northeast-3',version='1.2-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.3\-1 | inference, training | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-2 | inference, training | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-1 | inference, training | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.0\-1 | inference, training | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/xgboost:<tag> | 1 | inference, training | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-2 | inference, training | 
| 867004704886\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-1 | inference, training | 