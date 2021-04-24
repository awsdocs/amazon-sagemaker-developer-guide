# Docker Registry Paths and Example Code for Europe \(Milan\) \(eu\-south\-1\)<a name="ecr-eu-south-1"></a>

The following topics list parameters for each of the algorithms and deep learning containers in this region provided by Amazon SageMaker\.

**Topics**
+ [BlazingText \(algorithm\)](#blazingtext-eu-south-1.title)
+ [Chainer \(DLC\)](#chainer-eu-south-1.title)
+ [Clarify \(algorithm\)](#clarify-eu-south-1.title)
+ [Debugger \(algorithm\)](#debugger-eu-south-1.title)
+ [DeepAR Forecasting \(algorithm\)](#forecasting-deepar-eu-south-1.title)
+ [Factorization Machines \(algorithm\)](#factorization-machines-eu-south-1.title)
+ [Hugging Face \(algorithm\)](#huggingface-eu-south-1.title)
+ [IP Insights \(algorithm\)](#ipinsights-eu-south-1.title)
+ [Image classification \(algorithm\)](#image-classification-eu-south-1.title)
+ [Inferentia MXNet \(DLC\)](#inferentia-mxnet-eu-south-1.title)
+ [Inferentia PyTorch \(DLC\)](#inferentia-pytorch-eu-south-1.title)
+ [K\-Means \(algorithm\)](#kmeans-eu-south-1.title)
+ [KNN \(algorithm\)](#knn-eu-south-1.title)
+ [Linear Learner \(algorithm\)](#linear-learner-eu-south-1.title)
+ [MXNet \(DLC\)](#mxnet-eu-south-1.title)
+ [MXNet Coach \(DLC\)](#coach-mxnet-eu-south-1.title)
+ [Model Monitor \(algorithm\)](#model-monitor-eu-south-1.title)
+ [NTM \(algorithm\)](#ntm-eu-south-1.title)
+ [Neo Image Classification \(algorithm\)](#image-classification-neo-eu-south-1.title)
+ [Neo MXNet \(DLC\)](#neo-mxnet-eu-south-1.title)
+ [Neo PyTorch \(DLC\)](#neo-pytorch-eu-south-1.title)
+ [Neo Tensorflow \(DLC\)](#neo-tensorflow-eu-south-1.title)
+ [Neo XGBoost \(algorithm\)](#xgboost-neo-eu-south-1.title)
+ [Object Detection \(algorithm\)](#object-detection-eu-south-1.title)
+ [Object2Vec \(algorithm\)](#object2vec-eu-south-1.title)
+ [PCA \(algorithm\)](#pca-eu-south-1.title)
+ [PyTorch \(DLC\)](#pytorch-eu-south-1.title)
+ [Random Cut Forest \(algorithm\)](#randomcutforest-eu-south-1.title)
+ [Scikit\-learn \(algorithm\)](#sklearn-eu-south-1.title)
+ [Semantic Segmentation \(algorithm\)](#semantic-segmentation-eu-south-1.title)
+ [Seq2Seq \(algorithm\)](#seq2seq-eu-south-1.title)
+ [Spark \(algorithm\)](#spark-eu-south-1.title)
+ [SparkML Serving \(algorithm\)](#sparkml-serving-eu-south-1.title)
+ [Tensorflow \(DLC\)](#tensorflow-eu-south-1.title)
+ [Tensorflow Coach \(DLC\)](#coach-tensorflow-eu-south-1.title)
+ [Tensorflow Inferentia \(DLC\)](#inferentia-tensorflow-eu-south-1.title)
+ [Tensorflow Ray \(DLC\)](#ray-tensorflow-eu-south-1.title)
+ [XGBoost \(algorithm\)](#xgboost-eu-south-1.title)

## BlazingText \(algorithm\)<a name="blazingtext-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='blazingtext',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/blazingtext:<tag> | 1 | inference, training | 

## Chainer \(DLC\)<a name="chainer-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='chainer',region='eu-south-1',version='5.0.0',py_version='py3',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-chainer:<tag> | 4\.0\.0 | inference, training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-chainer:<tag> | 4\.1\.0 | inference, training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-chainer:<tag> | 5\.0\.0 | inference, training | CPU, GPU | py2, py3 | 

## Clarify \(algorithm\)<a name="clarify-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='clarify',region='eu-south-1',version='1.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 638885417683\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:<tag> | 1\.0 | processing | 

## Debugger \(algorithm\)<a name="debugger-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='debugger',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 563282790590\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-debugger\-rules:<tag> | latest | debugger | 

## DeepAR Forecasting \(algorithm\)<a name="forecasting-deepar-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='forecasting-deepar',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/forecasting\-deepar:<tag> | 1 | inference, training | 

## Factorization Machines \(algorithm\)<a name="factorization-machines-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='factorization-machines',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/factorization\-machines:<tag> | 1 | inference, training | 

## Hugging Face \(algorithm\)<a name="huggingface-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='huggingface',region='eu-south-1',version='4.4.2',image_scope='training',base_framework_version='tensorflow2.4.1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.4\.2 | training | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.4\.2 | training | 

## IP Insights \(algorithm\)<a name="ipinsights-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ipinsights',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/ipinsights:<tag> | 1 | inference, training | 

## Image classification \(algorithm\)<a name="image-classification-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/image\-classification:<tag> | 1 | inference, training | 

## Inferentia MXNet \(DLC\)<a name="inferentia-mxnet-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-mxnet',region='eu-south-1',version='1.5.1',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-neo\-mxnet:<tag> | 1\.5\.1 | inference | inf | py3 | 

## Inferentia PyTorch \(DLC\)<a name="inferentia-pytorch-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-pytorch',region='eu-south-1',version='1.5.1',py_version='py3')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-neo\-pytorch:<tag> | 1\.5\.1 | inference | inf | py3 | 

## K\-Means \(algorithm\)<a name="kmeans-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='kmeans',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/kmeans:<tag> | 1 | inference, training | 

## KNN \(algorithm\)<a name="knn-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='knn',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/knn:<tag> | 1 | inference, training | 

## Linear Learner \(algorithm\)<a name="linear-learner-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='linear-learner',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/linear\-learner:<tag> | 1 | inference, training | 

## MXNet \(DLC\)<a name="mxnet-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='mxnet',region='eu-south-1',version='1.4.1',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet\-eia:<tag> | 1\.3\.0 | eia | CPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet\-serving\-eia:<tag> | 1\.4\.0 | eia | CPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet\-serving:<tag> | 1\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet\-serving:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 0\.12\.1 | inference | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 0\.12\.1 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.2\.1 | inference | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.2\.1 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.3\.0 | inference | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.3\.0 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.4\.1 | eia | CPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.5\.1 | eia | CPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.7\.0 | eia | CPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.4\.1 | inference | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.7\.0 | inference | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.4\.1 | training | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.7\.0 | training | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py37 | 

## MXNet Coach \(DLC\)<a name="coach-mxnet-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-mxnet',region='eu-south-1',version='0.11',py_version='py3',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-mxnet:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-mxnet:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 

## Model Monitor \(algorithm\)<a name="model-monitor-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='model-monitor',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 933208885752\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-model\-monitor\-analyzer:<tag> |  | monitoring | 

## NTM \(algorithm\)<a name="ntm-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ntm',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/ntm:<tag> | 1 | inference, training | 

## Neo Image Classification \(algorithm\)<a name="image-classification-neo-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification-neo',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/image\-classification\-neo:<tag> | latest | inference | 

## Neo MXNet \(DLC\)<a name="neo-mxnet-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-mxnet',region='eu-south-1',version='1.8',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-inference\-mxnet:<tag> | 1\.8 | inference | CPU, GPU | py3 | 

## Neo PyTorch \(DLC\)<a name="neo-pytorch-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-pytorch',region='eu-south-1',version='1.6',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.4 | inference | CPU, GPU | py3 | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.5 | inference | CPU, GPU | py3 | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.6 | inference | CPU, GPU | py3 | 

## Neo Tensorflow \(DLC\)<a name="neo-tensorflow-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-tensorflow',region='eu-south-1',version='1.15.3',py_version='py3',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-inference\-tensorflow:<tag> | 1\.15\.3 | inference | CPU, GPU | py3 | 

## Neo XGBoost \(algorithm\)<a name="xgboost-neo-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost-neo',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/xgboost\-neo:<tag> | latest | inference | 

## Object Detection \(algorithm\)<a name="object-detection-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object-detection',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/object\-detection:<tag> | 1 | inference, training | 

## Object2Vec \(algorithm\)<a name="object2vec-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object2vec',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/object2vec:<tag> | 1 | inference, training | 

## PCA \(algorithm\)<a name="pca-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pca',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pca:<tag> | 1 | inference, training | 

## PyTorch \(DLC\)<a name="pytorch-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pytorch',region='eu-south-1',version='1.8.0',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 0\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 0\.4\.0 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-inference\-eia:<tag> | 1\.3\.1 | eia | CPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.2\.0 | inference | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.3\.1 | inference | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.4\.0 | inference | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.5\.0 | inference | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py3, py36 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.7\.1 | inference | CPU, GPU | py3, py36 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py3, py36 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.1 | inference | CPU, GPU | py3, py36 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.2\.0 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.3\.1 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.5\.0 | training | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py3, py36 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.7\.1 | training | CPU, GPU | py3, py36 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py3, py36 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.1 | training | CPU, GPU | py3, py36 | 

## Random Cut Forest \(algorithm\)<a name="randomcutforest-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='randomcutforest',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/randomcutforest:<tag> | 1 | inference, training | 

## Scikit\-learn \(algorithm\)<a name="sklearn-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sklearn',region='eu-south-1',version='0.23-1',image_scope='inference')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 978288397137\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.20\.0 | inference, training | 
| 978288397137\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.23\-1 | inference, training | 

## Semantic Segmentation \(algorithm\)<a name="semantic-segmentation-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='semantic-segmentation',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/semantic\-segmentation:<tag> | 1 | inference, training | 

## Seq2Seq \(algorithm\)<a name="seq2seq-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='seq2seq',region='eu-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/seq2seq:<tag> | 1 | inference, training | 

## Spark \(algorithm\)<a name="spark-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='spark',region='eu-south-1',version='3.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 753923664805\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 2\.4 | processing | 
| 753923664805\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 3\.0 | processing | 

## SparkML Serving \(algorithm\)<a name="sparkml-serving-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sparkml-serving',region='eu-south-1',version='2.4')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 978288397137\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-sparkml\-serving:<tag> | 2\.2 | inference | 
| 978288397137\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-sparkml\-serving:<tag> | 2\.4 | inference | 

## Tensorflow \(DLC\)<a name="tensorflow-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='tensorflow',region='eu-south-1',version='1.12.0',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-eia:<tag> | 1\.10\.0 | eia | CPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.11\.0 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.12\.0 | training | CPU, GPU | py2, py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.13\.1 | training | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.11\.0 | eia | CPU | \- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.12\.0 | eia | CPU | \- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.13\.0 | eia | CPU | \- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving:<tag> | 1\.11\.0 | inference | CPU, GPU | \- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving:<tag> | 1\.12\.0 | inference | CPU, GPU | \- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.10\.0 | inference | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.10\.0 | training | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.5\.0 | inference | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.5\.0 | training | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.6\.0 | inference | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.6\.0 | training | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.7\.0 | inference | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.7\.0 | training | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.8\.0 | inference | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.8\.0 | training | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.9\.0 | inference | CPU, GPU | py2 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.9\.0 | training | CPU, GPU | py2 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.14\.0 | eia | CPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.15\.0 | eia | CPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.0\.0 | eia | CPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.3\.0 | eia | CPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.13\.0 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.14\.0 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.0 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.2 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.3 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.4 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.5 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.0 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.1 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.2 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.3 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.4 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.0 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.1 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.2 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.3 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.0 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.1 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.2 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.0 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.1 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.2 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.1 | inference | CPU, GPU | \- | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.13\.1 | training | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.14\.0 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.0 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.2 | training | CPU, GPU | py2, py3, py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.3 | training | CPU, GPU | py2, py3, py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.4 | training | CPU, GPU | py3, py36, py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.5 | training | CPU, GPU | py3, py36, py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.0 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.1 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.2 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.3 | training | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.4 | training | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.0 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.1 | training | CPU, GPU | py2, py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.2 | training | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.3 | training | CPU, GPU | py3 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.0 | training | CPU, GPU | py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.1 | training | CPU, GPU | py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.2 | training | CPU, GPU | py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.0 | training | CPU, GPU | py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.1 | training | CPU, GPU | py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.2 | training | CPU, GPU | py37 | 
| 692866216735\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.1 | training | CPU, GPU | py37 | 

## Tensorflow Coach \(DLC\)<a name="coach-tensorflow-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-tensorflow',region='eu-south-1',version='1.0.0',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.10\-<tag> | 0\.10 | training | CPU, GPU | py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.10\.1\-<tag> | 0\.10\.1 | training | CPU, GPU | py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\.1\-<tag> | 0\.11\.1 | training | CPU, GPU | py3 | 

## Tensorflow Inferentia \(DLC\)<a name="inferentia-tensorflow-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-tensorflow',region='eu-south-1',version='1.15.0',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 966458181534\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-neo\-tensorflow:<tag> | 1\.15\.0 | inference | inf | py3 | 

## Tensorflow Ray \(DLC\)<a name="ray-tensorflow-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ray-tensorflow',region='eu-south-1',version='0.8.5',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.5\-<tag> | 0\.5 | training | CPU, GPU | py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.5\.3\-<tag> | 0\.5\.3 | training | CPU, GPU | py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.6\-<tag> | 0\.6 | training | CPU, GPU | py3 | 
| 048378556238\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.6\.5\-<tag> | 0\.6\.5 | training | CPU, GPU | py3 | 

## XGBoost \(algorithm\)<a name="xgboost-eu-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost',region='eu-south-1',version='1.2-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 257386234256\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/xgboost:<tag> | 1 | inference, training | 
| 978288397137\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-1 | inference, training | 
| 978288397137\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-2 | inference, training | 
| 978288397137\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.0\-1 | inference, training | 
| 978288397137\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-1 | inference, training | 