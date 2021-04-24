# Docker Registry Paths and Example Code for Middle East \(Bahrain\) \(me\-south\-1\)<a name="ecr-me-south-1"></a>

The following topics list parameters for each of the algorithms and deep learning containers in this region provided by Amazon SageMaker\.

**Topics**
+ [BlazingText \(algorithm\)](#blazingtext-me-south-1.title)
+ [Chainer \(DLC\)](#chainer-me-south-1.title)
+ [Clarify \(algorithm\)](#clarify-me-south-1.title)
+ [Debugger \(algorithm\)](#debugger-me-south-1.title)
+ [DeepAR Forecasting \(algorithm\)](#forecasting-deepar-me-south-1.title)
+ [Factorization Machines \(algorithm\)](#factorization-machines-me-south-1.title)
+ [Hugging Face \(algorithm\)](#huggingface-me-south-1.title)
+ [IP Insights \(algorithm\)](#ipinsights-me-south-1.title)
+ [Image classification \(algorithm\)](#image-classification-me-south-1.title)
+ [Inferentia MXNet \(DLC\)](#inferentia-mxnet-me-south-1.title)
+ [Inferentia PyTorch \(DLC\)](#inferentia-pytorch-me-south-1.title)
+ [K\-Means \(algorithm\)](#kmeans-me-south-1.title)
+ [KNN \(algorithm\)](#knn-me-south-1.title)
+ [Linear Learner \(algorithm\)](#linear-learner-me-south-1.title)
+ [MXNet \(DLC\)](#mxnet-me-south-1.title)
+ [MXNet Coach \(DLC\)](#coach-mxnet-me-south-1.title)
+ [Model Monitor \(algorithm\)](#model-monitor-me-south-1.title)
+ [NTM \(algorithm\)](#ntm-me-south-1.title)
+ [Neo Image Classification \(algorithm\)](#image-classification-neo-me-south-1.title)
+ [Neo MXNet \(DLC\)](#neo-mxnet-me-south-1.title)
+ [Neo PyTorch \(DLC\)](#neo-pytorch-me-south-1.title)
+ [Neo Tensorflow \(DLC\)](#neo-tensorflow-me-south-1.title)
+ [Neo XGBoost \(algorithm\)](#xgboost-neo-me-south-1.title)
+ [Object Detection \(algorithm\)](#object-detection-me-south-1.title)
+ [Object2Vec \(algorithm\)](#object2vec-me-south-1.title)
+ [PCA \(algorithm\)](#pca-me-south-1.title)
+ [PyTorch \(DLC\)](#pytorch-me-south-1.title)
+ [Random Cut Forest \(algorithm\)](#randomcutforest-me-south-1.title)
+ [Scikit\-learn \(algorithm\)](#sklearn-me-south-1.title)
+ [Semantic Segmentation \(algorithm\)](#semantic-segmentation-me-south-1.title)
+ [Seq2Seq \(algorithm\)](#seq2seq-me-south-1.title)
+ [Spark \(algorithm\)](#spark-me-south-1.title)
+ [SparkML Serving \(algorithm\)](#sparkml-serving-me-south-1.title)
+ [Tensorflow \(DLC\)](#tensorflow-me-south-1.title)
+ [Tensorflow Coach \(DLC\)](#coach-tensorflow-me-south-1.title)
+ [Tensorflow Inferentia \(DLC\)](#inferentia-tensorflow-me-south-1.title)
+ [Tensorflow Ray \(DLC\)](#ray-tensorflow-me-south-1.title)
+ [XGBoost \(algorithm\)](#xgboost-me-south-1.title)

## BlazingText \(algorithm\)<a name="blazingtext-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='blazingtext',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/blazingtext:<tag> | 1 | inference, training | 

## Chainer \(DLC\)<a name="chainer-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='chainer',region='me-south-1',version='5.0.0',py_version='py3',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-chainer:<tag> | 4\.0\.0 | inference, training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-chainer:<tag> | 4\.1\.0 | inference, training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-chainer:<tag> | 5\.0\.0 | inference, training | CPU, GPU | py2, py3 | 

## Clarify \(algorithm\)<a name="clarify-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='clarify',region='me-south-1',version='1.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 835444307964\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:<tag> | 1\.0 | processing | 

## Debugger \(algorithm\)<a name="debugger-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='debugger',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 986000313247\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-debugger\-rules:<tag> | latest | debugger | 

## DeepAR Forecasting \(algorithm\)<a name="forecasting-deepar-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='forecasting-deepar',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/forecasting\-deepar:<tag> | 1 | inference, training | 

## Factorization Machines \(algorithm\)<a name="factorization-machines-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='factorization-machines',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/factorization\-machines:<tag> | 1 | inference, training | 

## Hugging Face \(algorithm\)<a name="huggingface-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='huggingface',region='me-south-1',version='4.4.2',image_scope='training',base_framework_version='tensorflow2.4.1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.4\.2 | training | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.4\.2 | training | 

## IP Insights \(algorithm\)<a name="ipinsights-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ipinsights',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/ipinsights:<tag> | 1 | inference, training | 

## Image classification \(algorithm\)<a name="image-classification-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/image\-classification:<tag> | 1 | inference, training | 

## Inferentia MXNet \(DLC\)<a name="inferentia-mxnet-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-mxnet',region='me-south-1',version='1.5.1',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-neo\-mxnet:<tag> | 1\.5\.1 | inference | inf | py3 | 

## Inferentia PyTorch \(DLC\)<a name="inferentia-pytorch-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-pytorch',region='me-south-1',version='1.5.1',py_version='py3')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-neo\-pytorch:<tag> | 1\.5\.1 | inference | inf | py3 | 

## K\-Means \(algorithm\)<a name="kmeans-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='kmeans',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/kmeans:<tag> | 1 | inference, training | 

## KNN \(algorithm\)<a name="knn-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='knn',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/knn:<tag> | 1 | inference, training | 

## Linear Learner \(algorithm\)<a name="linear-learner-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='linear-learner',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/linear\-learner:<tag> | 1 | inference, training | 

## MXNet \(DLC\)<a name="mxnet-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='mxnet',region='me-south-1',version='1.4.1',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.4\.1 | eia | CPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.5\.1 | eia | CPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.7\.0 | eia | CPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.4\.1 | inference | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.7\.0 | inference | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.4\.1 | training | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.7\.0 | training | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py37 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet\-eia:<tag> | 1\.3\.0 | eia | CPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet\-serving\-eia:<tag> | 1\.4\.0 | eia | CPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet\-serving:<tag> | 1\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet\-serving:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 0\.12\.1 | inference | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 0\.12\.1 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.2\.1 | inference | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.2\.1 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.3\.0 | inference | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.3\.0 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 

## MXNet Coach \(DLC\)<a name="coach-mxnet-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-mxnet',region='me-south-1',version='0.11',py_version='py3',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-mxnet:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-mxnet:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 

## Model Monitor \(algorithm\)<a name="model-monitor-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='model-monitor',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 607024016150\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-model\-monitor\-analyzer:<tag> |  | monitoring | 

## NTM \(algorithm\)<a name="ntm-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ntm',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/ntm:<tag> | 1 | inference, training | 

## Neo Image Classification \(algorithm\)<a name="image-classification-neo-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification-neo',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/image\-classification\-neo:<tag> | latest | inference | 

## Neo MXNet \(DLC\)<a name="neo-mxnet-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-mxnet',region='me-south-1',version='1.8',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-inference\-mxnet:<tag> | 1\.8 | inference | CPU, GPU | py3 | 

## Neo PyTorch \(DLC\)<a name="neo-pytorch-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-pytorch',region='me-south-1',version='1.6',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.4 | inference | CPU, GPU | py3 | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.5 | inference | CPU, GPU | py3 | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.6 | inference | CPU, GPU | py3 | 

## Neo Tensorflow \(DLC\)<a name="neo-tensorflow-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-tensorflow',region='me-south-1',version='1.15.3',py_version='py3',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-inference\-tensorflow:<tag> | 1\.15\.3 | inference | CPU, GPU | py3 | 

## Neo XGBoost \(algorithm\)<a name="xgboost-neo-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost-neo',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/xgboost\-neo:<tag> | latest | inference | 

## Object Detection \(algorithm\)<a name="object-detection-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object-detection',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/object\-detection:<tag> | 1 | inference, training | 

## Object2Vec \(algorithm\)<a name="object2vec-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object2vec',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/object2vec:<tag> | 1 | inference, training | 

## PCA \(algorithm\)<a name="pca-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pca',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pca:<tag> | 1 | inference, training | 

## PyTorch \(DLC\)<a name="pytorch-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pytorch',region='me-south-1',version='1.8.0',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-inference\-eia:<tag> | 1\.3\.1 | eia | CPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.2\.0 | inference | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.3\.1 | inference | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.4\.0 | inference | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.5\.0 | inference | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py3, py36 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.7\.1 | inference | CPU, GPU | py3, py36 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py3, py36 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.1 | inference | CPU, GPU | py3, py36 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.2\.0 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.3\.1 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.5\.0 | training | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py3, py36 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.7\.1 | training | CPU, GPU | py3, py36 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py3, py36 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.1 | training | CPU, GPU | py3, py36 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 0\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 0\.4\.0 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 

## Random Cut Forest \(algorithm\)<a name="randomcutforest-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='randomcutforest',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/randomcutforest:<tag> | 1 | inference, training | 

## Scikit\-learn \(algorithm\)<a name="sklearn-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sklearn',region='me-south-1',version='0.23-1',image_scope='inference')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 801668240914\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.20\.0 | inference, training | 
| 801668240914\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.23\-1 | inference, training | 

## Semantic Segmentation \(algorithm\)<a name="semantic-segmentation-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='semantic-segmentation',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/semantic\-segmentation:<tag> | 1 | inference, training | 

## Seq2Seq \(algorithm\)<a name="seq2seq-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='seq2seq',region='me-south-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/seq2seq:<tag> | 1 | inference, training | 

## Spark \(algorithm\)<a name="spark-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='spark',region='me-south-1',version='3.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 750251592176\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 2\.4 | processing | 
| 750251592176\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 3\.0 | processing | 

## SparkML Serving \(algorithm\)<a name="sparkml-serving-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sparkml-serving',region='me-south-1',version='2.4')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 801668240914\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-sparkml\-serving:<tag> | 2\.2 | inference | 
| 801668240914\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-sparkml\-serving:<tag> | 2\.4 | inference | 

## Tensorflow \(DLC\)<a name="tensorflow-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='tensorflow',region='me-south-1',version='1.12.0',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.14\.0 | eia | CPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.15\.0 | eia | CPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.0\.0 | eia | CPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.3\.0 | eia | CPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.13\.0 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.14\.0 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.0 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.2 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.3 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.4 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.5 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.0 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.1 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.2 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.3 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.4 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.0 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.1 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.2 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.3 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.0 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.1 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.2 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.0 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.1 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.2 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.1 | inference | CPU, GPU | \- | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.13\.1 | training | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.14\.0 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.0 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.2 | training | CPU, GPU | py2, py3, py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.3 | training | CPU, GPU | py2, py3, py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.4 | training | CPU, GPU | py3, py36, py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.5 | training | CPU, GPU | py3, py36, py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.0 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.1 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.2 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.3 | training | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.4 | training | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.0 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.1 | training | CPU, GPU | py2, py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.2 | training | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.3 | training | CPU, GPU | py3 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.0 | training | CPU, GPU | py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.1 | training | CPU, GPU | py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.2 | training | CPU, GPU | py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.0 | training | CPU, GPU | py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.1 | training | CPU, GPU | py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.2 | training | CPU, GPU | py37 | 
| 217643126080\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.1 | training | CPU, GPU | py37 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-eia:<tag> | 1\.10\.0 | eia | CPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.11\.0 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.12\.0 | training | CPU, GPU | py2, py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.13\.1 | training | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.11\.0 | eia | CPU | \- | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.12\.0 | eia | CPU | \- | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.13\.0 | eia | CPU | \- | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving:<tag> | 1\.11\.0 | inference | CPU, GPU | \- | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving:<tag> | 1\.12\.0 | inference | CPU, GPU | \- | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.10\.0 | inference | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.10\.0 | training | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.5\.0 | inference | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.5\.0 | training | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.6\.0 | inference | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.6\.0 | training | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.7\.0 | inference | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.7\.0 | training | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.8\.0 | inference | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.8\.0 | training | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.9\.0 | inference | CPU, GPU | py2 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.9\.0 | training | CPU, GPU | py2 | 

## Tensorflow Coach \(DLC\)<a name="coach-tensorflow-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-tensorflow',region='me-south-1',version='1.0.0',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.10\-<tag> | 0\.10 | training | CPU, GPU | py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.10\.1\-<tag> | 0\.10\.1 | training | CPU, GPU | py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\.1\-<tag> | 0\.11\.1 | training | CPU, GPU | py3 | 

## Tensorflow Inferentia \(DLC\)<a name="inferentia-tensorflow-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-tensorflow',region='me-south-1',version='1.15.0',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 836785723513\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-neo\-tensorflow:<tag> | 1\.15\.0 | inference | inf | py3 | 

## Tensorflow Ray \(DLC\)<a name="ray-tensorflow-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ray-tensorflow',region='me-south-1',version='0.8.5',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.5\-<tag> | 0\.5 | training | CPU, GPU | py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.5\.3\-<tag> | 0\.5\.3 | training | CPU, GPU | py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.6\-<tag> | 0\.6 | training | CPU, GPU | py3 | 
| 724002660598\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.6\.5\-<tag> | 0\.6\.5 | training | CPU, GPU | py3 | 

## XGBoost \(algorithm\)<a name="xgboost-me-south-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost',region='me-south-1',version='1.2-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/xgboost:<tag> | 1 | inference, training | 
| 801668240914\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-1 | inference, training | 
| 801668240914\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-2 | inference, training | 
| 801668240914\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.0\-1 | inference, training | 
| 801668240914\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-1 | inference, training | 