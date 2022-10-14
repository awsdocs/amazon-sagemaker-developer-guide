# Docker Registry Paths and Example Code for Asia Pacific \(Hong Kong\) \(ap\-east\-1\)<a name="ecr-ap-east-1"></a>

The following topics list parameters for each of the algorithms and deep learning containers in this region provided by Amazon SageMaker\.

**Topics**
+ [AutoGluon \(algorithm\)](#autogluon-ap-east-1.title)
+ [BlazingText \(algorithm\)](#blazingtext-ap-east-1.title)
+ [Chainer \(DLC\)](#chainer-ap-east-1.title)
+ [Clarify \(algorithm\)](#clarify-ap-east-1.title)
+ [Data Wrangler \(algorithm\)](#data-wrangler-ap-east-1.title)
+ [Debugger \(algorithm\)](#debugger-ap-east-1.title)
+ [DeepAR Forecasting \(algorithm\)](#forecasting-deepar-ap-east-1.title)
+ [Factorization Machines \(algorithm\)](#factorization-machines-ap-east-1.title)
+ [Hugging Face \(algorithm\)](#huggingface-ap-east-1.title)
+ [IP Insights \(algorithm\)](#ipinsights-ap-east-1.title)
+ [Image classification \(algorithm\)](#image-classification-ap-east-1.title)
+ [Inferentia MXNet \(DLC\)](#inferentia-mxnet-ap-east-1.title)
+ [Inferentia PyTorch \(DLC\)](#inferentia-pytorch-ap-east-1.title)
+ [K\-Means \(algorithm\)](#kmeans-ap-east-1.title)
+ [KNN \(algorithm\)](#knn-ap-east-1.title)
+ [Linear Learner \(algorithm\)](#linear-learner-ap-east-1.title)
+ [MXNet \(DLC\)](#mxnet-ap-east-1.title)
+ [MXNet Coach \(DLC\)](#coach-mxnet-ap-east-1.title)
+ [Model Monitor \(algorithm\)](#model-monitor-ap-east-1.title)
+ [NTM \(algorithm\)](#ntm-ap-east-1.title)
+ [Neo Image Classification \(algorithm\)](#image-classification-neo-ap-east-1.title)
+ [Neo MXNet \(DLC\)](#neo-mxnet-ap-east-1.title)
+ [Neo PyTorch \(DLC\)](#neo-pytorch-ap-east-1.title)
+ [Neo Tensorflow \(DLC\)](#neo-tensorflow-ap-east-1.title)
+ [Neo XGBoost \(algorithm\)](#xgboost-neo-ap-east-1.title)
+ [Object Detection \(algorithm\)](#object-detection-ap-east-1.title)
+ [Object2Vec \(algorithm\)](#object2vec-ap-east-1.title)
+ [PCA \(algorithm\)](#pca-ap-east-1.title)
+ [PyTorch \(DLC\)](#pytorch-ap-east-1.title)
+ [Random Cut Forest \(algorithm\)](#randomcutforest-ap-east-1.title)
+ [Scikit\-learn \(algorithm\)](#sklearn-ap-east-1.title)
+ [Semantic Segmentation \(algorithm\)](#semantic-segmentation-ap-east-1.title)
+ [Seq2Seq \(algorithm\)](#seq2seq-ap-east-1.title)
+ [Spark \(algorithm\)](#spark-ap-east-1.title)
+ [SparkML Serving \(algorithm\)](#sparkml-serving-ap-east-1.title)
+ [Tensorflow \(DLC\)](#tensorflow-ap-east-1.title)
+ [Tensorflow Coach \(DLC\)](#coach-tensorflow-ap-east-1.title)
+ [Tensorflow Inferentia \(DLC\)](#inferentia-tensorflow-ap-east-1.title)
+ [Tensorflow Ray \(DLC\)](#ray-tensorflow-ap-east-1.title)
+ [XGBoost \(algorithm\)](#xgboost-ap-east-1.title)

## AutoGluon \(algorithm\)<a name="autogluon-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='autogluon',region='ap-east-1',image_scope='inference',version='0.4')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-training:<tag> | 0\.5\.2 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-inference:<tag> | 0\.5\.2 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-training:<tag> | 0\.4\.3 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-inference:<tag> | 0\.4\.3 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-training:<tag> | 0\.4\.2 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-inference:<tag> | 0\.4\.2 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-training:<tag> | 0\.4\.0 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-inference:<tag> | 0\.4\.0 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-training:<tag> | 0\.3\.2 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-inference:<tag> | 0\.3\.2 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-training:<tag> | 0\.3\.1 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/autogluon\-inference:<tag> | 0\.3\.1 | inference | 

## BlazingText \(algorithm\)<a name="blazingtext-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='blazingtext',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/blazingtext:<tag> | 1 | inference, training | 

## Chainer \(DLC\)<a name="chainer-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='chainer',region='ap-east-1',version='5.0.0',py_version='py3',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-chainer:<tag> | 5\.0\.0 | inference, training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-chainer:<tag> | 4\.1\.0 | inference, training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-chainer:<tag> | 4\.0\.0 | inference, training | CPU, GPU | py2, py3 | 

## Clarify \(algorithm\)<a name="clarify-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='clarify',region='ap-east-1',version='1.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 098760798382\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-clarify\-processing:<tag> | 1\.0 | processing | 

## Data Wrangler \(algorithm\)<a name="data-wrangler-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='data-wrangler',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 707077482487\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-data\-wrangler\-container:<tag> | 1\.x | processing | 

## Debugger \(algorithm\)<a name="debugger-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='debugger',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 199566480951\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-debugger\-rules:<tag> | latest | debugger | 

## DeepAR Forecasting \(algorithm\)<a name="forecasting-deepar-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='forecasting-deepar',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/forecasting\-deepar:<tag> | 1 | inference, training | 

## Factorization Machines \(algorithm\)<a name="factorization-machines-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='factorization-machines',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/factorization\-machines:<tag> | 1 | inference, training | 

## Hugging Face \(algorithm\)<a name="huggingface-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='huggingface',region='ap-east-1',version='4.4.2',image_scope='training',base_framework_version='tensorflow2.4.1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.17\.0 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.17\.0 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.17\.0 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.17\.0 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.12\.3 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.12\.3 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.12\.3 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.12\.3 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.11\.0 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.11\.0 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.11\.0 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.11\.0 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.6\.1 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-inference:<tag> | 4\.6\.1 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-inference:<tag> | 4\.6\.1 | inference | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.5\.0 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.5\.0 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-pytorch\-training:<tag> | 4\.4\.2 | training | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/huggingface\-tensorflow\-training:<tag> | 4\.4\.2 | training | 

## IP Insights \(algorithm\)<a name="ipinsights-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ipinsights',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/ipinsights:<tag> | 1 | inference, training | 

## Image classification \(algorithm\)<a name="image-classification-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/image\-classification:<tag> | 1 | inference, training | 

## Inferentia MXNet \(DLC\)<a name="inferentia-mxnet-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-mxnet',region='ap-east-1',version='1.5.1',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-neo\-mxnet:<tag> | 1\.8 | inference | inf | py3 | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-neo\-mxnet:<tag> | 1\.5\.1 | inference | inf | py3 | 

## Inferentia PyTorch \(DLC\)<a name="inferentia-pytorch-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-pytorch',region='ap-east-1',version='1.9',py_version='py3')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-neo\-pytorch:<tag> | 1\.9 | inference | inf | py3 | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-neo\-pytorch:<tag> | 1\.8 | inference | inf | py3 | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-neo\-pytorch:<tag> | 1\.7 | inference | inf | py3 | 

## K\-Means \(algorithm\)<a name="kmeans-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='kmeans',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/kmeans:<tag> | 1 | inference, training | 

## KNN \(algorithm\)<a name="knn-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='knn',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/knn:<tag> | 1 | inference, training | 

## Linear Learner \(algorithm\)<a name="linear-learner-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='linear-learner',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/linear\-learner:<tag> | 1 | inference, training | 

## MXNet \(DLC\)<a name="mxnet-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='mxnet',region='ap-east-1',version='1.4.1',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.9\.0 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.9\.0 | inference | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.7\.0 | training | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.7\.0 | inference | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.7\.0 | eia | CPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.5\.1 | eia | CPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-training:<tag> | 1\.4\.1 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet\-serving:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-inference:<tag> | 1\.4\.1 | inference | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/mxnet\-inference\-eia:<tag> | 1\.4\.1 | eia | CPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet\-serving:<tag> | 1\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet\-serving\-eia:<tag> | 1\.4\.0 | eia | CPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.3\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.3\.0 | inference | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet\-eia:<tag> | 1\.3\.0 | eia | CPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.2\.1 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.2\.1 | inference | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 0\.12\.1 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-mxnet:<tag> | 0\.12\.1 | inference | CPU, GPU | py2, py3 | 

## MXNet Coach \(DLC\)<a name="coach-mxnet-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-mxnet',region='ap-east-1',version='0.11',py_version='py3',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-mxnet:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-mxnet:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 

## Model Monitor \(algorithm\)<a name="model-monitor-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='model-monitor',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 001633400207\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-model\-monitor\-analyzer:<tag> |  | monitoring | 

## NTM \(algorithm\)<a name="ntm-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ntm',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/ntm:<tag> | 1 | inference, training | 

## Neo Image Classification \(algorithm\)<a name="image-classification-neo-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification-neo',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/image\-classification\-neo:<tag> | latest | inference | 

## Neo MXNet \(DLC\)<a name="neo-mxnet-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-mxnet',region='ap-east-1',version='1.8',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-inference\-mxnet:<tag> | 1\.8 | inference | CPU, GPU | py3 | 

## Neo PyTorch \(DLC\)<a name="neo-pytorch-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-pytorch',region='ap-east-1',version='1.6',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.8 | inference | CPU, GPU | py3 | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.7 | inference | CPU, GPU | py3 | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.6 | inference | CPU, GPU | py3 | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.5 | inference | CPU, GPU | py3 | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-inference\-pytorch:<tag> | 1\.4 | inference | CPU, GPU | py3 | 

## Neo Tensorflow \(DLC\)<a name="neo-tensorflow-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-tensorflow',region='ap-east-1',version='1.15.3',py_version='py3',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-inference\-tensorflow:<tag> | 2\.4\.2 | inference | CPU, GPU | py3 | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-inference\-tensorflow:<tag> | 1\.15\.3 | inference | CPU, GPU | py3 | 

## Neo XGBoost \(algorithm\)<a name="xgboost-neo-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost-neo',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/xgboost\-neo:<tag> | latest | inference | 

## Object Detection \(algorithm\)<a name="object-detection-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object-detection',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/object\-detection:<tag> | 1 | inference, training | 

## Object2Vec \(algorithm\)<a name="object2vec-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object2vec',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/object2vec:<tag> | 1 | inference, training | 

## PCA \(algorithm\)<a name="pca-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pca',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pca:<tag> | 1 | inference, training | 

## PyTorch \(DLC\)<a name="pytorch-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pytorch',region='ap-east-1',version='1.8.0',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.12\.0 | inference | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.12\.0 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.11\.0 | inference | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.11\.0 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.10\.2 | inference | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.10\.2 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.10\.0 | inference | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.10\.0 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.9\.1 | inference | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.9\.1 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.9\.0 | inference | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.9\.0 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.1 | inference | CPU, GPU | py3, py36 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.1 | training | CPU, GPU | py3, py36 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py3, py36 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py3, py36 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.7\.1 | inference | CPU, GPU | py3, py36 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.7\.1 | training | CPU, GPU | py3, py36 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py3, py36 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py3, py36 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.5\.0 | inference | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.5\.0 | training | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.4\.0 | inference | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.3\.1 | inference | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.3\.1 | training | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-inference:<tag> | 1\.2\.0 | inference | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/pytorch\-training:<tag> | 1\.2\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 0\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-pytorch:<tag> | 0\.4\.0 | training | CPU, GPU | py2, py3 | 

## Random Cut Forest \(algorithm\)<a name="randomcutforest-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='randomcutforest',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/randomcutforest:<tag> | 1 | inference, training | 

## Scikit\-learn \(algorithm\)<a name="sklearn-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sklearn',region='ap-east-1',version='0.23-1',image_scope='inference')
```


| Registry path | Version | Package version | Job types \(image scope\) | 
| --- | --- | --- | --- | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 1\.0\-1 | 1\.0\.2 | inference, training | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.23\-1 | 0\.23\.2 | inference, training | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-scikit\-learn:<tag> | 0\.20\.0 | 0\.20\.0 | inference, training | 

## Semantic Segmentation \(algorithm\)<a name="semantic-segmentation-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='semantic-segmentation',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/semantic\-segmentation:<tag> | 1 | inference, training | 

## Seq2Seq \(algorithm\)<a name="seq2seq-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='seq2seq',region='ap-east-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/seq2seq:<tag> | 1 | inference, training | 

## Spark \(algorithm\)<a name="spark-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='spark',region='ap-east-1',version='3.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 732049463269\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 3\.1 | processing | 
| 732049463269\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 3\.0 | processing | 
| 732049463269\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-spark\-processing:<tag> | 2\.4 | processing | 

## SparkML Serving \(algorithm\)<a name="sparkml-serving-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sparkml-serving',region='ap-east-1',version='2.4')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-sparkml\-serving:<tag> | 2\.4 | inference | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-sparkml\-serving:<tag> | 2\.2 | inference | 

## Tensorflow \(DLC\)<a name="tensorflow-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='tensorflow',region='ap-east-1',version='1.12.0',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.9\.1 | training | CPU, GPU | py39 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.8\.0 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.8\.0 | training | CPU, GPU | py39 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.7\.1 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.7\.0 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.6\.3 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.6\.3 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.6\.2 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.6\.0 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.6\.0 | training | CPU, GPU | py38 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.5\.1 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.5\.1 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.5\.0 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.3 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.3 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.4\.1 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.4\.1 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.2 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.2 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.1 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.1 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.3\.0 | eia | CPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.3\.0 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.3\.0 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.2 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.2 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.1 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.1 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.2\.0 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.2\.0 | training | CPU, GPU | py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.3 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.3 | training | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.2 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.2 | training | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.1 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.1 | training | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.1\.0 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.1\.0 | training | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.4 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.4 | training | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.3 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.3 | training | CPU, GPU | py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.2 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.2 | training | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.1 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.1 | training | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 2\.0\.0 | eia | CPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 2\.0\.0 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 2\.0\.0 | training | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.5 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.5 | training | CPU, GPU | py3, py36, py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.4 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.4 | training | CPU, GPU | py3, py36, py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.3 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.3 | training | CPU, GPU | py2, py3, py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.2 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.2 | training | CPU, GPU | py2, py3, py37 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.15\.0 | eia | CPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.15\.0 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.15\.0 | training | CPU, GPU | py2, py3 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference\-eia:<tag> | 1\.14\.0 | eia | CPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.14\.0 | inference | CPU, GPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.14\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.13\.1 | training | CPU, GPU | py2 | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-training:<tag> | 1\.13\.1 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.13\.0 | eia | CPU | \- | 
| 871362719292\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/tensorflow\-inference:<tag> | 1\.13\.0 | inference | CPU, GPU | \- | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.12\.0 | eia | CPU | \- | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving:<tag> | 1\.12\.0 | inference | CPU, GPU | \- | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.12\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.11\.0 | eia | CPU | \- | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow\-serving:<tag> | 1\.11\.0 | inference | CPU, GPU | \- | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.11\.0 | training | CPU, GPU | py2, py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow\-eia:<tag> | 1\.10\.0 | eia | CPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.10\.0 | inference | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.10\.0 | training | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.9\.0 | inference | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.9\.0 | training | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.8\.0 | inference | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.8\.0 | training | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.7\.0 | inference | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.7\.0 | training | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.6\.0 | inference | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.6\.0 | training | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.5\.0 | inference | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.5\.0 | training | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-tensorflow:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 

## Tensorflow Coach \(DLC\)<a name="coach-tensorflow-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-tensorflow',region='ap-east-1',version='1.0.0',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\.1\-<tag> | 0\.11\.1 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.10\.1\-<tag> | 0\.10\.1 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:coach0\.10\-<tag> | 0\.10 | training | CPU, GPU | py3 | 

## Tensorflow Inferentia \(DLC\)<a name="inferentia-tensorflow-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-tensorflow',region='ap-east-1',version='1.15.0',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-neo\-tensorflow:<tag> | 2\.5\.2 | inference | inf | py3 | 
| 110948597952\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-neo\-tensorflow:<tag> | 1\.15\.0 | inference | inf | py3 | 

## Tensorflow Ray \(DLC\)<a name="ray-tensorflow-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ray-tensorflow',region='ap-east-1',version='0.8.5',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.6\.5\-<tag> | 0\.6\.5 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.6\-<tag> | 0\.6 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.5\.3\-<tag> | 0\.5\.3 | training | CPU, GPU | py3 | 
| 057415533634\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-rl\-tensorflow:ray0\.5\-<tag> | 0\.5 | training | CPU, GPU | py3 | 

## XGBoost \(algorithm\)<a name="xgboost-ap-east-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost',region='ap-east-1',version='1.5-1')
```


| Registry path | Version | Package version | Job types \(image scope\) | 
| --- | --- | --- | --- | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.5\-1 | 1\.5\.2 | inference, training | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.3\-1 | 1\.3\.3 | inference, training | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-2 | 1\.2\.0 | inference, training | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.2\-1 | 1\.2\.0 | inference, training | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 1\.0\-1 | 1\.0 | inference, training | 
| 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/xgboost:<tag> | 1 | 0\.72 | inference, training | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-2 | 0\.90 | inference, training | 
| 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-xgboost:<tag> | 0\.90\-1 | 0\.90 | inference, training | 