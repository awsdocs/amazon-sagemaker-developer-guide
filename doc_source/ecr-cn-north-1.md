# Docker Registry Paths and Example Code for China \(Beijing\) \(cn\-north\-1\)<a name="ecr-cn-north-1"></a>

The following topics list parameters for each of the algorithms and deep learning containers in this region provided by Amazon SageMaker\.

**Topics**
+ [AutoGluon \(algorithm\)](#autogluon-cn-north-1.title)
+ [BlazingText \(algorithm\)](#blazingtext-cn-north-1.title)
+ [Chainer \(DLC\)](#chainer-cn-north-1.title)
+ [Clarify \(algorithm\)](#clarify-cn-north-1.title)
+ [Data Wrangler \(algorithm\)](#data-wrangler-cn-north-1.title)
+ [Debugger \(algorithm\)](#debugger-cn-north-1.title)
+ [DeepAR Forecasting \(algorithm\)](#forecasting-deepar-cn-north-1.title)
+ [Factorization Machines \(algorithm\)](#factorization-machines-cn-north-1.title)
+ [Hugging Face \(algorithm\)](#huggingface-cn-north-1.title)
+ [IP Insights \(algorithm\)](#ipinsights-cn-north-1.title)
+ [Image classification \(algorithm\)](#image-classification-cn-north-1.title)
+ [Inferentia MXNet \(DLC\)](#inferentia-mxnet-cn-north-1.title)
+ [Inferentia PyTorch \(DLC\)](#inferentia-pytorch-cn-north-1.title)
+ [K\-Means \(algorithm\)](#kmeans-cn-north-1.title)
+ [KNN \(algorithm\)](#knn-cn-north-1.title)
+ [Linear Learner \(algorithm\)](#linear-learner-cn-north-1.title)
+ [MXNet \(DLC\)](#mxnet-cn-north-1.title)
+ [MXNet Coach \(DLC\)](#coach-mxnet-cn-north-1.title)
+ [Model Monitor \(algorithm\)](#model-monitor-cn-north-1.title)
+ [NTM \(algorithm\)](#ntm-cn-north-1.title)
+ [Neo Image Classification \(algorithm\)](#image-classification-neo-cn-north-1.title)
+ [Neo MXNet \(DLC\)](#neo-mxnet-cn-north-1.title)
+ [Neo PyTorch \(DLC\)](#neo-pytorch-cn-north-1.title)
+ [Neo Tensorflow \(DLC\)](#neo-tensorflow-cn-north-1.title)
+ [Neo XGBoost \(algorithm\)](#xgboost-neo-cn-north-1.title)
+ [Object Detection \(algorithm\)](#object-detection-cn-north-1.title)
+ [Object2Vec \(algorithm\)](#object2vec-cn-north-1.title)
+ [PCA \(algorithm\)](#pca-cn-north-1.title)
+ [PyTorch \(DLC\)](#pytorch-cn-north-1.title)
+ [Random Cut Forest \(algorithm\)](#randomcutforest-cn-north-1.title)
+ [Scikit\-learn \(algorithm\)](#sklearn-cn-north-1.title)
+ [Semantic Segmentation \(algorithm\)](#semantic-segmentation-cn-north-1.title)
+ [Seq2Seq \(algorithm\)](#seq2seq-cn-north-1.title)
+ [Spark \(algorithm\)](#spark-cn-north-1.title)
+ [SparkML Serving \(algorithm\)](#sparkml-serving-cn-north-1.title)
+ [Tensorflow \(DLC\)](#tensorflow-cn-north-1.title)
+ [Tensorflow Coach \(DLC\)](#coach-tensorflow-cn-north-1.title)
+ [Tensorflow Inferentia \(DLC\)](#inferentia-tensorflow-cn-north-1.title)
+ [Tensorflow Ray \(DLC\)](#ray-tensorflow-cn-north-1.title)
+ [XGBoost \(algorithm\)](#xgboost-cn-north-1.title)

## AutoGluon \(algorithm\)<a name="autogluon-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='autogluon',region='cn-north-1',image_scope='inference',version='0.4')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/autogluon\-inference:<tag> | 0\.4\.0 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/autogluon\-inference:<tag> | 0\.3\.2 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/autogluon\-inference:<tag> | 0\.3\.1 | inference | 

## BlazingText \(algorithm\)<a name="blazingtext-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='blazingtext',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/blazingtext:<tag> | 1 | inference, training | 

## Chainer \(DLC\)<a name="chainer-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='chainer',region='cn-north-1',version='5.0.0',py_version='py3',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-chainer:<tag> | 5\.0\.0 | inference, training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-chainer:<tag> | 4\.1\.0 | inference, training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-chainer:<tag> | 4\.0\.0 | inference, training | CPU, GPU | py2, py3 | 

## Clarify \(algorithm\)<a name="clarify-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='clarify',region='cn-north-1',version='1.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 122526803553\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-clarify\-processing:<tag> | 1\.0 | processing | 

## Data Wrangler \(algorithm\)<a name="data-wrangler-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='data-wrangler',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 245909111842\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-data\-wrangler\-container:<tag> | 1\.x | processing | 

## Debugger \(algorithm\)<a name="debugger-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='debugger',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 618459771430\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-debugger\-rules:<tag> | latest | debugger | 

## DeepAR Forecasting \(algorithm\)<a name="forecasting-deepar-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='forecasting-deepar',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/forecasting\-deepar:<tag> | 1 | inference, training | 

## Factorization Machines \(algorithm\)<a name="factorization-machines-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='factorization-machines',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/factorization\-machines:<tag> | 1 | inference, training | 

## Hugging Face \(algorithm\)<a name="huggingface-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='huggingface',region='cn-north-1',version='4.4.2',image_scope='training',base_framework_version='tensorflow2.4.1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.17\.0 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-training:<tag> | 4\.17\.0 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-inference:<tag> | 4\.17\.0 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-inference:<tag> | 4\.17\.0 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.12\.3 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-training:<tag> | 4\.12\.3 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-inference:<tag> | 4\.12\.3 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-inference:<tag> | 4\.12\.3 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.11\.0 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-training:<tag> | 4\.11\.0 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-inference:<tag> | 4\.11\.0 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-inference:<tag> | 4\.11\.0 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.10\.2 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-training:<tag> | 4\.10\.2 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-inference:<tag> | 4\.10\.2 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-inference:<tag> | 4\.10\.2 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.6\.1 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-training:<tag> | 4\.6\.1 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-inference:<tag> | 4\.6\.1 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-inference:<tag> | 4\.6\.1 | inference | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.5\.0 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-training:<tag> | 4\.5\.0 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-pytorch\-training:<tag> | 4\.4\.2 | training | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/huggingface\-tensorflow\-training:<tag> | 4\.4\.2 | training | 

## IP Insights \(algorithm\)<a name="ipinsights-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ipinsights',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/ipinsights:<tag> | 1 | inference, training | 

## Image classification \(algorithm\)<a name="image-classification-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/image\-classification:<tag> | 1 | inference, training | 

## Inferentia MXNet \(DLC\)<a name="inferentia-mxnet-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-mxnet',region='cn-north-1',version='1.5.1',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-neo\-mxnet:<tag> | 1\.8 | inference | inf | py3 | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-neo\-mxnet:<tag> | 1\.5\.1 | inference | inf | py3 | 

## Inferentia PyTorch \(DLC\)<a name="inferentia-pytorch-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-pytorch',region='cn-north-1',version='1.9',py_version='py3')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-neo\-pytorch:<tag> | 1\.9 | inference | inf | py3 | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-neo\-pytorch:<tag> | 1\.8 | inference | inf | py3 | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-neo\-pytorch:<tag> | 1\.7 | inference | inf | py3 | 

## K\-Means \(algorithm\)<a name="kmeans-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='kmeans',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/kmeans:<tag> | 1 | inference, training | 

## KNN \(algorithm\)<a name="knn-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='knn',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/knn:<tag> | 1 | inference, training | 

## Linear Learner \(algorithm\)<a name="linear-learner-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='linear-learner',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/linear\-learner:<tag> | 1 | inference, training | 

## MXNet \(DLC\)<a name="mxnet-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='mxnet',region='cn-north-1',version='1.4.1',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-training:<tag> | 1\.9\.0 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-inference:<tag> | 1\.9\.0 | inference | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-training:<tag> | 1\.7\.0 | training | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-inference:<tag> | 1\.7\.0 | inference | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-inference\-eia:<tag> | 1\.7\.0 | eia | CPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-inference\-eia:<tag> | 1\.5\.1 | eia | CPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-training:<tag> | 1\.4\.1 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet\-serving:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-inference:<tag> | 1\.4\.1 | inference | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/mxnet\-inference\-eia:<tag> | 1\.4\.1 | eia | CPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet\-serving:<tag> | 1\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet\-serving\-eia:<tag> | 1\.4\.0 | eia | CPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.3\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.3\.0 | inference | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet\-eia:<tag> | 1\.3\.0 | eia | CPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.2\.1 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.2\.1 | inference | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 0\.12\.1 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-mxnet:<tag> | 0\.12\.1 | inference | CPU, GPU | py2, py3 | 

## MXNet Coach \(DLC\)<a name="coach-mxnet-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-mxnet',region='cn-north-1',version='0.11',py_version='py3',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-mxnet:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-mxnet:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 

## Model Monitor \(algorithm\)<a name="model-monitor-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='model-monitor',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 453000072557\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-model\-monitor\-analyzer:<tag> |  | monitoring | 

## NTM \(algorithm\)<a name="ntm-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ntm',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/ntm:<tag> | 1 | inference, training | 

## Neo Image Classification \(algorithm\)<a name="image-classification-neo-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='image-classification-neo',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/image\-classification\-neo:<tag> | latest | inference | 

## Neo MXNet \(DLC\)<a name="neo-mxnet-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-mxnet',region='cn-north-1',version='1.8',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-inference\-mxnet:<tag> | 1\.8 | inference | CPU, GPU | py3 | 

## Neo PyTorch \(DLC\)<a name="neo-pytorch-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-pytorch',region='cn-north-1',version='1.6',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-inference\-pytorch:<tag> | 1\.8 | inference | CPU, GPU | py3 | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-inference\-pytorch:<tag> | 1\.7 | inference | CPU, GPU | py3 | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-inference\-pytorch:<tag> | 1\.6 | inference | CPU, GPU | py3 | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-inference\-pytorch:<tag> | 1\.5 | inference | CPU, GPU | py3 | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-inference\-pytorch:<tag> | 1\.4 | inference | CPU, GPU | py3 | 

## Neo Tensorflow \(DLC\)<a name="neo-tensorflow-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='neo-tensorflow',region='cn-north-1',version='1.15.3',py_version='py3',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-inference\-tensorflow:<tag> | 2\.4\.2 | inference | CPU, GPU | py3 | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-inference\-tensorflow:<tag> | 1\.15\.3 | inference | CPU, GPU | py3 | 

## Neo XGBoost \(algorithm\)<a name="xgboost-neo-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost-neo',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/xgboost\-neo:<tag> | latest | inference | 

## Object Detection \(algorithm\)<a name="object-detection-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object-detection',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/object\-detection:<tag> | 1 | inference, training | 

## Object2Vec \(algorithm\)<a name="object2vec-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='object2vec',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/object2vec:<tag> | 1 | inference, training | 

## PCA \(algorithm\)<a name="pca-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pca',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pca:<tag> | 1 | inference, training | 

## PyTorch \(DLC\)<a name="pytorch-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='pytorch',region='cn-north-1',version='1.8.0',py_version='py3',image_scope='inference', instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.11\.0 | inference | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.11\.0 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.10\.2 | inference | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.10\.2 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.10\.0 | inference | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.10\.0 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.9\.1 | inference | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.9\.1 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.9\.0 | inference | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.9\.0 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.8\.1 | inference | CPU, GPU | py3, py36 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.8\.1 | training | CPU, GPU | py3, py36 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.8\.0 | inference | CPU, GPU | py3, py36 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.8\.0 | training | CPU, GPU | py3, py36 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.7\.1 | inference | CPU, GPU | py3, py36 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.7\.1 | training | CPU, GPU | py3, py36 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.6\.0 | inference | CPU, GPU | py3, py36 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.6\.0 | training | CPU, GPU | py3, py36 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.5\.0 | inference | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.5\.0 | training | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.4\.0 | inference | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.4\.0 | training | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference\-eia:<tag> | 1\.3\.1 | eia | CPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.3\.1 | inference | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.3\.1 | training | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-inference:<tag> | 1\.2\.0 | inference | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/pytorch\-training:<tag> | 1\.2\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-pytorch:<tag> | 1\.1\.0 | inference | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-pytorch:<tag> | 1\.1\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-pytorch:<tag> | 1\.0\.0 | inference | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-pytorch:<tag> | 1\.0\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-pytorch:<tag> | 0\.4\.0 | inference | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-pytorch:<tag> | 0\.4\.0 | training | CPU, GPU | py2, py3 | 

## Random Cut Forest \(algorithm\)<a name="randomcutforest-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='randomcutforest',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/randomcutforest:<tag> | 1 | inference, training | 

## Scikit\-learn \(algorithm\)<a name="sklearn-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sklearn',region='cn-north-1',version='0.23-1',image_scope='inference')
```


| Registry path | Version | Package version | Job types \(image scope\) | 
| --- | --- | --- | --- | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-scikit\-learn:<tag> | 1\.0\-1 | 1\.0\.2 | inference, training | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-scikit\-learn:<tag> | 0\.23\-1 | 0\.23\.2 | inference, training | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-scikit\-learn:<tag> | 0\.20\.0 | 0\.20\.0 | inference, training | 

## Semantic Segmentation \(algorithm\)<a name="semantic-segmentation-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='semantic-segmentation',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/semantic\-segmentation:<tag> | 1 | inference, training | 

## Seq2Seq \(algorithm\)<a name="seq2seq-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='seq2seq',region='cn-north-1')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/seq2seq:<tag> | 1 | inference, training | 

## Spark \(algorithm\)<a name="spark-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='spark',region='cn-north-1',version='3.0',image_scope='processing')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 671472414489\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-spark\-processing:<tag> | 3\.1 | processing | 
| 671472414489\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-spark\-processing:<tag> | 3\.0 | processing | 
| 671472414489\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-spark\-processing:<tag> | 2\.4 | processing | 

## SparkML Serving \(algorithm\)<a name="sparkml-serving-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='sparkml-serving',region='cn-north-1',version='2.4')
```


| Registry path | Version | Job types \(image scope\) | 
| --- | --- | --- | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-sparkml\-serving:<tag> | 2\.4 | inference | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-sparkml\-serving:<tag> | 2\.2 | inference | 

## Tensorflow \(DLC\)<a name="tensorflow-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='tensorflow',region='cn-north-1',version='1.12.0',image_scope='inference',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.8\.0 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.8\.0 | training | CPU, GPU | py39 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.7\.1 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.7\.0 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.6\.3 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.6\.3 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.6\.2 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.6\.0 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.6\.0 | training | CPU, GPU | py38 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.5\.1 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.5\.1 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.5\.0 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.4\.3 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.4\.3 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.4\.1 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.4\.1 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.3\.2 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.3\.2 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.3\.1 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.3\.1 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference\-eia:<tag> | 2\.3\.0 | eia | CPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.3\.0 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.3\.0 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.2\.2 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.2\.2 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.2\.1 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.2\.1 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.2\.0 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.2\.0 | training | CPU, GPU | py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.1\.3 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.1\.3 | training | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.1\.2 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.1\.2 | training | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.1\.1 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.1\.1 | training | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.1\.0 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.1\.0 | training | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.0\.4 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.0\.4 | training | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.0\.3 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.0\.3 | training | CPU, GPU | py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.0\.2 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.0\.2 | training | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.0\.1 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.0\.1 | training | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference\-eia:<tag> | 2\.0\.0 | eia | CPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 2\.0\.0 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 2\.0\.0 | training | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 1\.15\.5 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 1\.15\.5 | training | CPU, GPU | py3, py36, py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 1\.15\.4 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 1\.15\.4 | training | CPU, GPU | py3, py36, py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 1\.15\.3 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 1\.15\.3 | training | CPU, GPU | py2, py3, py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 1\.15\.2 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 1\.15\.2 | training | CPU, GPU | py2, py3, py37 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference\-eia:<tag> | 1\.15\.0 | eia | CPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 1\.15\.0 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 1\.15\.0 | training | CPU, GPU | py2, py3 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference\-eia:<tag> | 1\.14\.0 | eia | CPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 1\.14\.0 | inference | CPU, GPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 1\.14\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.13\.1 | training | CPU, GPU | py2 | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-training:<tag> | 1\.13\.1 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.13\.0 | eia | CPU | \- | 
| 727897471807\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/tensorflow\-inference:<tag> | 1\.13\.0 | inference | CPU, GPU | \- | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.12\.0 | eia | CPU | \- | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow\-serving:<tag> | 1\.12\.0 | inference | CPU, GPU | \- | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.12\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow\-serving\-eia:<tag> | 1\.11\.0 | eia | CPU | \- | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow\-serving:<tag> | 1\.11\.0 | inference | CPU, GPU | \- | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow\-scriptmode:<tag> | 1\.11\.0 | training | CPU, GPU | py2, py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow\-eia:<tag> | 1\.10\.0 | eia | CPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.10\.0 | inference | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.10\.0 | training | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.9\.0 | inference | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.9\.0 | training | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.8\.0 | inference | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.8\.0 | training | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.7\.0 | inference | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.7\.0 | training | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.6\.0 | inference | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.6\.0 | training | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.5\.0 | inference | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.5\.0 | training | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.4\.1 | inference | CPU, GPU | py2 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-tensorflow:<tag> | 1\.4\.1 | training | CPU, GPU | py2 | 

## Tensorflow Coach \(DLC\)<a name="coach-tensorflow-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='coach-tensorflow',region='cn-north-1',version='1.0.0',image_scope='training',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-tensorflow:coach0\.11\.1\-<tag> | 0\.11\.1 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-tensorflow:coach0\.11\.0\-<tag> | 0\.11\.0 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-tensorflow:coach0\.11\-<tag> | 0\.11 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-tensorflow:coach0\.10\.1\-<tag> | 0\.10\.1 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-tensorflow:coach0\.10\-<tag> | 0\.10 | training | CPU, GPU | py3 | 

## Tensorflow Inferentia \(DLC\)<a name="inferentia-tensorflow-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='inferentia-tensorflow',region='cn-north-1',version='1.15.0',instance_type='ml.inf1.6xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-neo\-tensorflow:<tag> | 2\.5\.2 | inference | inf | py3 | 
| 472730292857\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-neo\-tensorflow:<tag> | 1\.15\.0 | inference | inf | py3 | 

## Tensorflow Ray \(DLC\)<a name="ray-tensorflow-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='ray-tensorflow',region='cn-north-1',version='0.8.5',instance_type='ml.c5.4xlarge')
```


| Registry path | Version | Job types \(image scope\) | Processor types | Python versions | 
| --- | --- | --- | --- | --- | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-tensorflow:ray0\.6\.5\-<tag> | 0\.6\.5 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-tensorflow:ray0\.6\-<tag> | 0\.6 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-tensorflow:ray0\.5\.3\-<tag> | 0\.5\.3 | training | CPU, GPU | py3 | 
| 422961961927\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-rl\-tensorflow:ray0\.5\-<tag> | 0\.5 | training | CPU, GPU | py3 | 

## XGBoost \(algorithm\)<a name="xgboost-cn-north-1.title"></a>

SageMaker Python SDK example to retrieve registry path\.

```
from sagemaker import image_uris
image_uris.retrieve(framework='xgboost',region='cn-north-1',version='1.2-1')
```


| Registry path | Version | Package version | Job types \(image scope\) | 
| --- | --- | --- | --- | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-xgboost:<tag> | 1\.5\-1 | 1\.5\.2 | inference, training | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-xgboost:<tag> | 1\.3\-1 | 1\.3\.3 | inference, training | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-xgboost:<tag> | 1\.2\-2 | 1\.2\.0 | inference, training | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-xgboost:<tag> | 1\.2\-1 | 1\.2\.0 | inference, training | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-xgboost:<tag> | 1\.0\-1 | 1\.0 | inference, training | 
| 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/xgboost:<tag> | 1 | 1\.5\.2 | inference, training | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-xgboost:<tag> | 0\.90\-2 | 0\.90 | inference, training | 
| 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-xgboost:<tag> | 0\.90\-1 | 0\.90 | inference, training | 