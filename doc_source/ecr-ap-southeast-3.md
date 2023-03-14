# Docker Registry Paths and Example Code for Asia Pacific \(Jakarta\) \(ap\-southeast\-3\)<a name="ecr-ap-southeast-3"></a>

The following topics list parameters for each of the algorithms and deep learning containers in this region provided by Amazon SageMaker\.

**Topics**
+ [AutoGluon \(algorithm\)](#autogluon-ap-southeast-3.title)
+ [BlazingText \(algorithm\)](#blazingtext-ap-southeast-3.title)
+ [Clarify \(algorithm\)](#clarify-ap-southeast-3.title)
+ [DeepAR Forecasting \(algorithm\)](#forecasting-deepar-ap-southeast-3.title)
+ [Factorization Machines \(algorithm\)](#factorization-machines-ap-southeast-3.title)
+ [Hugging Face \(algorithm\)](#huggingface-ap-southeast-3.title)
+ [IP Insights \(algorithm\)](#ipinsights-ap-southeast-3.title)
+ [Image classification \(algorithm\)](#image-classification-ap-southeast-3.title)
+ [K\-Means \(algorithm\)](#kmeans-ap-southeast-3.title)
+ [KNN \(algorithm\)](#knn-ap-southeast-3.title)
+ [Linear Learner \(algorithm\)](#linear-learner-ap-southeast-3.title)
+ [MXNet \(DLC\)](#mxnet-ap-southeast-3.title)
+ [NTM \(algorithm\)](#ntm-ap-southeast-3.title)
+ [Object Detection \(algorithm\)](#object-detection-ap-southeast-3.title)
+ [Object2Vec \(algorithm\)](#object2vec-ap-southeast-3.title)
+ [PCA \(algorithm\)](#pca-ap-southeast-3.title)
+ [PyTorch \(DLC\)](#pytorch-ap-southeast-3.title)
+ [Random Cut Forest \(algorithm\)](#randomcutforest-ap-southeast-3.title)
+ [Scikit\-learn \(algorithm\)](#sklearn-ap-southeast-3.title)
+ [Semantic Segmentation \(algorithm\)](#semantic-segmentation-ap-southeast-3.title)
+ [Seq2Seq \(algorithm\)](#seq2seq-ap-southeast-3.title)
+ [Spark \(algorithm\)](#spark-ap-southeast-3.title)
+ [Tensorflow \(DLC\)](#tensorflow-ap-southeast-3.title)
+ [XGBoost \(algorithm\)](#xgboost-ap-southeast-3.title)

## AutoGluon \(algorithm\)<a name="autogluon-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='autogluon',region='ap-southeast-3',image_scope='inference',version='0.4')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-training:<tag> | 0\.5\.2 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-inference:<tag> | 0\.5\.2 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-training:<tag> | 0\.4\.3 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-inference:<tag> | 0\.4\.3 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-training:<tag> | 0\.4\.2 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-inference:<tag> | 0\.4\.2 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-training:<tag> | 0\.4\.0 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-inference:<tag> | 0\.4\.0 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-training:<tag> | 0\.3\.2 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-inference:<tag> | 0\.3\.2 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-training:<tag> | 0\.3\.1 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/autogluon\-inference:<tag> | 0\.3\.1 | inference | 

## BlazingText \(algorithm\)<a name="blazingtext-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='blazingtext',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/blazingtext:<tag> | 1 | inference, training | 

## Clarify \(algorithm\)<a name="clarify-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='clarify',region='ap-southeast-3',version='1.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 705930551576\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-clarify\-processing:<tag> | 1\.0 | processing | 

## DeepAR Forecasting \(algorithm\)<a name="forecasting-deepar-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='forecasting-deepar',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/forecasting\-deepar:<tag> | 1 | inference, training | 

## Factorization Machines \(algorithm\)<a name="factorization-machines-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='factorization-machines',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/factorization\-machines:<tag> | 1 | inference, training | 

## Hugging Face \(algorithm\)<a name="huggingface-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='huggingface',region='ap-southeast-3',version='4.4.2',image_scope='training',base_framework_version='tensorflow2.4.1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.17\.0 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.17\.0 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.17\.0 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.17\.0 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.12\.3 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.12\.3 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.12\.3 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.12\.3 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.11\.0 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.11\.0 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.11\.0 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.11\.0 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.6\.1 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.6\.1 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.6\.1 | inference | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.5\.0 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.5\.0 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.4\.2 | training | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.4\.2 | training | 

## IP Insights \(algorithm\)<a name="ipinsights-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ipinsights',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/ipinsights:<tag> | 1 | inference, training | 

## Image classification \(algorithm\)<a name="image-classification-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/image\-classification:<tag> | 1 | inference, training | 

## K\-Means \(algorithm\)<a name="kmeans-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='kmeans',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/kmeans:<tag> | 1 | inference, training | 

## KNN \(algorithm\)<a name="knn-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='knn',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/knn:<tag> | 1 | inference, training | 

## Linear Learner \(algorithm\)<a name="linear-learner-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='linear-learner',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/linear\-learner:<tag> | 1 | inference, training | 

## MXNet \(DLC\)<a name="mxnet-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='mxnet',region='ap-southeast-3',version='1.4.1',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-training:<tag> | 1\.9\.0 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-inference:<tag> | 1\.9\.0 | inference | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-training:<tag> | 1\.7\.0 | training | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-inference:<tag> | 1\.7\.0 | inference | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.7\.0 | eia | CPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.5\.1 | eia | CPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-training:<tag> | 1\.4\.1 | training | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-inference:<tag> | 1\.4\.1 | inference | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.4\.1 | eia | CPU | py2, py3 | 

## NTM \(algorithm\)<a name="ntm-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ntm',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/ntm:<tag> | 1 | inference, training | 

## Object Detection \(algorithm\)<a name="object-detection-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object-detection',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/object\-detection:<tag> | 1 | inference, training | 

## Object2Vec \(algorithm\)<a name="object2vec-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object2vec',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/object2vec:<tag> | 1 | inference, training | 

## PCA \(algorithm\)<a name="pca-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pca',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pca:<tag> | 1 | inference, training | 

## PyTorch \(DLC\)<a name="pytorch-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pytorch',region='ap-southeast-3',version='1.8.0',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.12\.0 | inference | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.12\.0 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.11\.0 | inference | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.11\.0 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.10\.2 | inference | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.10\.2 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.10\.0 | inference | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.10\.0 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.9\.1 | inference | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.9\.1 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.9\.0 | inference | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.9\.0 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.1 | inference | CPU, GPU | py3, py36 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.1 | training | CPU, GPU | py3, py36 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py3, py36 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py3, py36 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.7\.1 | inference | CPU, GPU | py3, py36 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.7\.1 | training | CPU, GPU | py3, py36 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py3, py36 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py3, py36 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference\-eia:<tag> | 1\.5\.1 | eia | CPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.5\.0 | inference | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.5\.0 | training | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.4\.0 | inference | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference\-eia:<tag> | 1\.3\.1 | eia | CPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.3\.1 | inference | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.3\.1 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-inference:<tag> | 1\.2\.0 | inference | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/pytorch\-training:<tag> | 1\.2\.0 | training | CPU, GPU | py2, py3 | 

## Random Cut Forest \(algorithm\)<a name="randomcutforest-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='randomcutforest',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/randomcutforest:<tag> | 1 | inference, training | 

## Scikit\-learn \(algorithm\)<a name="sklearn-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sklearn',region='ap-southeast-3',version='0.23-1',image_scope='inference')
```


| Registry path | Version | Package version | Job types \(image scope\) | 
| --- | --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 1\.0\-1 | 1\.0\.2 | inference, training | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.23\-1 | 0\.23\.2 | inference, training | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.20\.0 | 0\.20\.0 | inference, training | 

## Semantic Segmentation \(algorithm\)<a name="semantic-segmentation-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='semantic-segmentation',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/semantic\-segmentation:<tag> | 1 | inference, training | 

## Seq2Seq \(algorithm\)<a name="seq2seq-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='seq2seq',region='ap-southeast-3')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/seq2seq:<tag> | 1 | inference, training | 

## Spark \(algorithm\)<a name="spark-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='spark',region='ap-southeast-3',version='3.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 732049463269\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 3\.1 | processing | 
| 732049463269\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 3\.0 | processing | 
| 732049463269\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 2\.4 | processing | 

## Tensorflow \(DLC\)<a name="tensorflow-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='tensorflow',region='ap-southeast-3',version='1.12.0',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.9\.1 | training | CPU, GPU | py39 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.8\.0 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.8\.0 | training | CPU, GPU | py39 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.7\.1 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.7\.0 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.6\.3 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.6\.3 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.6\.2 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.6\.0 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.6\.0 | training | CPU, GPU | py38 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.5\.1 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.5\.1 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.5\.0 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.3 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.3 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.1 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.1 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.2 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.2 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.1 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.1 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.3\.0 | eia | CPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.0 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.0 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.2 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.2 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.1 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.1 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.0 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.0 | training | CPU, GPU | py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.3 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.3 | training | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.2 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.2 | training | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.1 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.1 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.0 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.0 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.4 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.4 | training | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.3 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.3 | training | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.2 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.2 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.1 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.1 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.0\.0 | eia | CPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.0 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.0 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.5 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.5 | training | CPU, GPU | py3, py36, py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.4 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.4 | training | CPU, GPU | py3, py36, py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.3 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.3 | training | CPU, GPU | py2, py3, py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.2 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.2 | training | CPU, GPU | py2, py3, py37 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.15\.0 | eia | CPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.0 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.0 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.14\.0 | eia | CPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.14\.0 | inference | CPU, GPU | \- | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.14\.0 | training | CPU, GPU | py2, py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-training:<tag> | 1\.13\.1 | training | CPU, GPU | py3 | 
| 907027046896\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.13\.0 | inference | CPU, GPU | \- | 

## XGBoost \(algorithm\)<a name="xgboost-ap-southeast-3.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost',region='ap-southeast-3',version='1.5-1')
```


| Registry path | Version | Package version | Job types \(image scope\) | 
| --- | --- | --- | --- | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.5\-1 | 1\.5\.2 | inference, training | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.3\-1 | 1\.3\.3 | inference, training | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-2 | 1\.2\.0 | inference, training | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-1 | 1\.2\.0 | inference, training | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.0\-1 | 1\.0 | inference, training | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/xgboost:<tag> | 1 | 0\.72 | inference, training | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-2 | 0\.90 | inference, training | 
| 951798379941\.dkr\.ecr\.ap\-southeast\-3\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-1 | 0\.90 | inference, training | 