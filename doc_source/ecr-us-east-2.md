# Docker Registry Paths and Example Code for US East \(Ohio\) \(us\-east\-2\)<a name="ecr-us-east-2"></a>

The following topics list parameters for each of the algorithms and deep learning containers in this region provided by Amazon SageMaker\.

**Topics**
+ [AutoGluon \(algorithm\)](#autogluon-us-east-2.title)
+ [BlazingText \(algorithm\)](#blazingtext-us-east-2.title)
+ [Chainer \(DLC\)](#chainer-us-east-2.title)
+ [Clarify \(algorithm\)](#clarify-us-east-2.title)
+ [Data Wrangler \(algorithm\)](#data-wrangler-us-east-2.title)
+ [Debugger \(algorithm\)](#debugger-us-east-2.title)
+ [DeepAR Forecasting \(algorithm\)](#forecasting-deepar-us-east-2.title)
+ [Factorization Machines \(algorithm\)](#factorization-machines-us-east-2.title)
+ [Hugging Face \(algorithm\)](#huggingface-us-east-2.title)
+ [IP Insights \(algorithm\)](#ipinsights-us-east-2.title)
+ [Image classification \(algorithm\)](#image-classification-us-east-2.title)
+ [Inferentia MXNet \(DLC\)](#inferentia-mxnet-us-east-2.title)
+ [Inferentia PyTorch \(DLC\)](#inferentia-pytorch-us-east-2.title)
+ [K\-Means \(algorithm\)](#kmeans-us-east-2.title)
+ [KNN \(algorithm\)](#knn-us-east-2.title)
+ [LDA \(algorithm\)](#lda-us-east-2.title)
+ [Linear Learner \(algorithm\)](#linear-learner-us-east-2.title)
+ [MXNet \(DLC\)](#mxnet-us-east-2.title)
+ [MXNet Coach \(DLC\)](#coach-mxnet-us-east-2.title)
+ [Model Monitor \(algorithm\)](#model-monitor-us-east-2.title)
+ [NTM \(algorithm\)](#ntm-us-east-2.title)
+ [Neo Image Classification \(algorithm\)](#image-classification-neo-us-east-2.title)
+ [Neo MXNet \(DLC\)](#neo-mxnet-us-east-2.title)
+ [Neo PyTorch \(DLC\)](#neo-pytorch-us-east-2.title)
+ [Neo Tensorflow \(DLC\)](#neo-tensorflow-us-east-2.title)
+ [Neo XGBoost \(algorithm\)](#xgboost-neo-us-east-2.title)
+ [Object Detection \(algorithm\)](#object-detection-us-east-2.title)
+ [Object2Vec \(algorithm\)](#object2vec-us-east-2.title)
+ [PCA \(algorithm\)](#pca-us-east-2.title)
+ [PyTorch \(DLC\)](#pytorch-us-east-2.title)
+ [Random Cut Forest \(algorithm\)](#randomcutforest-us-east-2.title)
+ [Ray PyTorch \(DLC\)](#ray-pytorch-us-east-2.title)
+ [Scikit\-learn \(algorithm\)](#sklearn-us-east-2.title)
+ [Semantic Segmentation \(algorithm\)](#semantic-segmentation-us-east-2.title)
+ [Seq2Seq \(algorithm\)](#seq2seq-us-east-2.title)
+ [Spark \(algorithm\)](#spark-us-east-2.title)
+ [SparkML Serving \(algorithm\)](#sparkml-serving-us-east-2.title)
+ [Tensorflow \(DLC\)](#tensorflow-us-east-2.title)
+ [Tensorflow Coach \(DLC\)](#coach-tensorflow-us-east-2.title)
+ [Tensorflow Inferentia \(DLC\)](#inferentia-tensorflow-us-east-2.title)
+ [Tensorflow Ray \(DLC\)](#ray-tensorflow-us-east-2.title)
+ [VW \(algorithm\)](#vw-us-east-2.title)
+ [XGBoost \(algorithm\)](#xgboost-us-east-2.title)

## AutoGluon \(algorithm\)<a name="autogluon-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='autogluon',region='us-east-2',image_scope='inference')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/autogluon\-training:<tag> | 0\.3\.1 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/autogluon\-inference:<tag> | 0\.3\.1 | inference | 

## BlazingText \(algorithm\)<a name="blazingtext-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='blazingtext',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 825641698319\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/blazingtext:<tag> | 1 | inference, training | 

## Chainer \(DLC\)<a name="chainer-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='chainer',region='us-east-2',version='5.0.0',py_version='py3',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-chainer:<tag> | 5\.0\.0 | inference, training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-chainer:<tag> | 4\.1\.0 | inference, training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-chainer:<tag> | 4\.0\.0 | inference, training | CPU, GPU | py2, py3 | 

## Clarify \(algorithm\)<a name="clarify-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='clarify',region='us-east-2',version='1.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 211330385671\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-clarify\-processing:<tag> | 1\.0 | processing | 

## Data Wrangler \(algorithm\)<a name="data-wrangler-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='data-wrangler',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 415577184552\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-data\-wrangler\-container:<tag> | 1\.x | processing | 

## Debugger \(algorithm\)<a name="debugger-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='debugger',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 915447279597\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-debugger\-rules:<tag> | latest | debugger | 

## DeepAR Forecasting \(algorithm\)<a name="forecasting-deepar-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='forecasting-deepar',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 566113047672\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/forecasting\-deepar:<tag> | 1 | inference, training | 

## Factorization Machines \(algorithm\)<a name="factorization-machines-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='factorization-machines',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/factorization\-machines:<tag> | 1 | inference, training | 

## Hugging Face \(algorithm\)<a name="huggingface-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='huggingface',region='us-east-2',version='4.4.2',image_scope='training',base_framework_version='tensorflow2.4.1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.12\.3 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.12\.3 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.12\.3 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.12\.3 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.11\.0 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.11\.0 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.11\.0 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.11\.0 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.6\.1 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.6\.1 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.6\.1 | inference | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.5\.0 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.5\.0 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.4\.2 | training | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.4\.2 | training | 

## IP Insights \(algorithm\)<a name="ipinsights-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ipinsights',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/ipinsights:<tag> | 1 | inference, training | 

## Image classification \(algorithm\)<a name="image-classification-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 825641698319\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/image\-classification:<tag> | 1 | inference, training | 

## Inferentia MXNet \(DLC\)<a name="inferentia-mxnet-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-mxnet',region='us-east-2',version='1.5.1',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-neo\-mxnet:<tag> | 1\.5\.1 | inference | inf | py3 | 

## Inferentia PyTorch \(DLC\)<a name="inferentia-pytorch-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-pytorch',region='us-east-2',version='1.5.1',py_version='py3')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-neo\-pytorch:<tag> | 1\.5\.1 | inference | inf | py3 | 

## K\-Means \(algorithm\)<a name="kmeans-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='kmeans',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/kmeans:<tag> | 1 | inference, training | 

## KNN \(algorithm\)<a name="knn-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='knn',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/knn:<tag> | 1 | inference, training | 

## LDA \(algorithm\)<a name="lda-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='lda',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 999911452149\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/lda:<tag> | 1 | inference, training | 

## Linear Learner \(algorithm\)<a name="linear-learner-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='linear-learner',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/linear\-learner:<tag> | 1 | inference, training | 

## MXNet \(DLC\)<a name="mxnet-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='mxnet',region='us-east-2',version='1.4.1',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-training:<tag> | 1\.7\.0 | training | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-inference:<tag> | 1\.7\.0 | inference | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.7\.0 | eia | CPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.5\.1 | eia | CPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-training:<tag> | 1\.4\.1 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet\-serving:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-inference:<tag> | 1\.4\.1 | inference | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.4\.1 | eia | CPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet\-serving:<tag> | 1\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet\-serving\-eia:<tag> | 1\.4\.0 | eia | CPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.3\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.3\.0 | inference | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet\-eia:<tag> | 1\.3\.0 | eia | CPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.2\.1 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.2\.1 | inference | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 0\.12\.1 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-mxnet:<tag> | 0\.12\.1 | inference | CPU, GPU | py2, py3 | 

## MXNet Coach \(DLC\)<a name="coach-mxnet-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-mxnet',region='us-east-2',version='0.11',py_version='py3',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-mxnet:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-mxnet:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 

## Model Monitor \(algorithm\)<a name="model-monitor-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='model-monitor',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 777275614652\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-model\-monitor\-analyzer:<tag> |  | monitoring | 

## NTM \(algorithm\)<a name="ntm-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ntm',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/ntm:<tag> | 1 | inference, training | 

## Neo Image Classification \(algorithm\)<a name="image-classification-neo-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification-neo',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/image\-classification\-neo:<tag> | latest | inference | 

## Neo MXNet \(DLC\)<a name="neo-mxnet-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-mxnet',region='us-east-2',version='1.8',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-inference\-mxnet:<tag> | 1\.8 | inference | CPU, GPU | py3 | 

## Neo PyTorch \(DLC\)<a name="neo-pytorch-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-pytorch',region='us-east-2',version='1.6',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.6 | inference | CPU, GPU | py3 | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.5 | inference | CPU, GPU | py3 | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.4 | inference | CPU, GPU | py3 | 

## Neo Tensorflow \(DLC\)<a name="neo-tensorflow-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-tensorflow',region='us-east-2',version='1.15.3',py_version='py3',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-inference\-tensorflow:<tag> | 1\.15\.3 | inference | CPU, GPU | py3 | 

## Neo XGBoost \(algorithm\)<a name="xgboost-neo-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost-neo',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/xgboost\-neo:<tag> | latest | inference | 

## Object Detection \(algorithm\)<a name="object-detection-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object-detection',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 825641698319\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/object\-detection:<tag> | 1 | inference, training | 

## Object2Vec \(algorithm\)<a name="object2vec-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object2vec',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/object2vec:<tag> | 1 | inference, training | 

## PCA \(algorithm\)<a name="pca-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pca',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pca:<tag> | 1 | inference, training | 

## PyTorch \(DLC\)<a name="pytorch-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pytorch',region='us-east-2',version='1.8.0',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.9\.1 | inference | CPU, GPU | py38 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.9\.1 | training | CPU, GPU | py38 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.9\.0 | inference | CPU, GPU | py38 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.9\.0 | training | CPU, GPU | py38 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.1 | inference | CPU, GPU | py3, py36 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.1 | training | CPU, GPU | py3, py36 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py3, py36 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py3, py36 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.7\.1 | inference | CPU, GPU | py3, py36 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.7\.1 | training | CPU, GPU | py3, py36 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py3, py36 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py3, py36 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference\-eia:<tag> | 1\.5\.1 | eia | CPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.5\.0 | inference | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.5\.0 | training | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.4\.0 | inference | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference\-eia:<tag> | 1\.3\.1 | eia | CPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.3\.1 | inference | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.3\.1 | training | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-inference:<tag> | 1\.2\.0 | inference | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/pytorch\-training:<tag> | 1\.2\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-pytorch:<tag> | 0\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-pytorch:<tag> | 0\.4\.0 | training | CPU, GPU | py2, py3 | 

## Random Cut Forest \(algorithm\)<a name="randomcutforest-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='randomcutforest',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/randomcutforest:<tag> | 1 | inference, training | 

## Ray PyTorch \(DLC\)<a name="ray-pytorch-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ray-pytorch',region='us-east-2',version='0.8.5',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 462105765813\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-ray\-container:ray\-1\.6\.0\-torch\-<tag> | 1\.6\.0 | training | CPU, GPU | py36 | 
| 462105765813\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-ray\-container:ray\-0\.8\.5\-torch\-<tag> | 0\.8\.5 | training | CPU, GPU | py36 | 

## Scikit\-learn \(algorithm\)<a name="sklearn-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sklearn',region='us-east-2',version='0.23-1',image_scope='inference')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.23\-1 | inference, training | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.20\.0 | inference, training | 

## Semantic Segmentation \(algorithm\)<a name="semantic-segmentation-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='semantic-segmentation',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 825641698319\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/semantic\-segmentation:<tag> | 1 | inference, training | 

## Seq2Seq \(algorithm\)<a name="seq2seq-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='seq2seq',region='us-east-2')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 825641698319\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/seq2seq:<tag> | 1 | inference, training | 

## Spark \(algorithm\)<a name="spark-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='spark',region='us-east-2',version='3.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 314815235551\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 3\.0 | processing | 
| 314815235551\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 2\.4 | processing | 

## SparkML Serving \(algorithm\)<a name="sparkml-serving-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sparkml-serving',region='us-east-2',version='2.4')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-sparkml\-serving:<tag> | 2\.4 | inference | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-sparkml\-serving:<tag> | 2\.2 | inference | 

## Tensorflow \(DLC\)<a name="tensorflow-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='tensorflow',region='us-east-2',version='1.12.0',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.5\.1 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.5\.1 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.5\.0 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.3 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.3 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.1 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.1 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.2 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.2 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.1 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.1 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.3\.0 | eia | CPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.0 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.0 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.2 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.2 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.1 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.1 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.0 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.0 | training | CPU, GPU | py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.3 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.3 | training | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.2 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.2 | training | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.1 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.1 | training | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.0 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.0 | training | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.4 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.4 | training | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.3 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.3 | training | CPU, GPU | py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.2 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.2 | training | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.1 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.1 | training | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.0\.0 | eia | CPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.0 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.0 | training | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.5 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.5 | training | CPU, GPU | py3, py36, py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.4 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.4 | training | CPU, GPU | py3, py36, py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.3 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.3 | training | CPU, GPU | py2, py3, py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.2 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.2 | training | CPU, GPU | py2, py3, py37 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.15\.0 | eia | CPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.0 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.0 | training | CPU, GPU | py2, py3 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.14\.0 | eia | CPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.14\.0 | inference | CPU, GPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 1\.14\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.13\.1 | training | CPU, GPU | py2 | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-training:<tag> | 1\.13\.1 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.13\.0 | eia | CPU | \- | 
| 763104351884\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.13\.0 | inference | CPU, GPU | \- | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.12\.0 | eia | CPU | \- | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow\-serving:<tag> | 1\.12\.0 | inference | CPU, GPU | \- | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.12\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.11\.0 | eia | CPU | \- | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow\-serving:<tag> | 1\.11\.0 | inference | CPU, GPU | \- | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.11\.0 | training | CPU, GPU | py2, py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow\-eia:<tag> | 1\.10\.0 | eia | CPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.10\.0 | inference | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.10\.0 | training | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.9\.0 | inference | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.9\.0 | training | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.8\.0 | inference | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.8\.0 | training | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.7\.0 | inference | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.7\.0 | training | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.6\.0 | inference | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.6\.0 | training | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.5\.0 | inference | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.5\.0 | training | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 

## Tensorflow Coach \(DLC\)<a name="coach-tensorflow-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-tensorflow',region='us-east-2',version='1.0.0',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 462105765813\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-coach\-container:coach\-1\.0\.0\-tf\-<tag> | 1\.0\.0 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\.1\-<tag> | 0\.11\.1 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.10\.1\-<tag> | 0\.10\.1 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.10\-<tag> | 0\.10 | training | CPU, GPU | py3 | 

## Tensorflow Inferentia \(DLC\)<a name="inferentia-tensorflow-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-tensorflow',region='us-east-2',version='1.15.0',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 007439368137\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-neo\-tensorflow:<tag> | 1\.15\.0 | inference | inf | py3 | 

## Tensorflow Ray \(DLC\)<a name="ray-tensorflow-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ray-tensorflow',region='us-east-2',version='0.8.5',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 462105765813\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-ray\-container:ray\-1\.6\.0\-tf\-<tag> | 1\.6\.0 | training | CPU, GPU | py37 | 
| 462105765813\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-ray\-container:ray\-0\.8\.5\-tf\-<tag> | 0\.8\.5 | training | CPU, GPU | py36 | 
| 462105765813\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-ray\-container:ray\-0\.8\.2\-tf\-<tag> | 0\.8\.2 | training | CPU, GPU | py36 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.6\.5\-<tag> | 0\.6\.5 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.6\-<tag> | 0\.6 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.5\.3\-<tag> | 0\.5\.3 | training | CPU, GPU | py3 | 
| 520713654638\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.5\-<tag> | 0\.5 | training | CPU, GPU | py3 | 

## VW \(algorithm\)<a name="vw-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='vw',region='us-east-2',version='8.7.0',image_scope='training')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 462105765813\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-rl\-vw\-container:vw\-8\.7\.0\-<tag> | 8\.7\.0 | training | 

## XGBoost \(algorithm\)<a name="xgboost-us-east-2.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost',region='us-east-2',version='1.2-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.3\-1 | inference, training | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-2 | inference, training | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-1 | inference, training | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.0\-1 | inference, training | 
| 825641698319\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/xgboost:<tag> | 1 | inference, training | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-2 | inference, training | 
| 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-1 | inference, training | 