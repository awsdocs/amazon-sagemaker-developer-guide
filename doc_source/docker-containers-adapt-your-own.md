# Adapting your own Docker container to work with SageMaker<a name="docker-containers-adapt-your-own"></a>

You can adapt an existing Docker image to work with SageMaker\. You may need to use an existing, external Docker image with SageMaker when you have a container that satisfies feature or safety requirements that are not currently supported by a pre\-built SageMaker image\. There are two toolkits that allow you to bring your own container and adapt it to work with SageMaker:
+ [SageMaker Training Toolkit](https://github.com/aws/sagemaker-training-toolkit)
+ [SageMaker Inference Toolkit](https://github.com/aws/sagemaker-inference-toolkit)

The following topics show how to adapt your existing image using the SageMaker Training and Inference toolkits:

**Topics**
+ [Individual Framework Libraries](#docker-containers-adapt-your-own-frameworks)
+ [Using the SageMaker Training and Inference Toolkits](amazon-sagemaker-toolkits.md)
+ [Adapting your own training container](adapt-training-container.md)
+ [Adapting Your Own Inference Container](adapt-inference-container.md)

## Individual Framework Libraries<a name="docker-containers-adapt-your-own-frameworks"></a>

In addition to the SageMaker Training Toolkit and SageMaker Inference Toolkit, SageMaker also provides toolkits specialized for TensorFlow, MXNet, PyTorch, and Chainer\. The following table provides links to the GitHub repositories that contain the source code for each framework and their respective serving toolkits\. The instructions linked are for using the Python SDK to run training algorithms and host models on SageMaker\. The functionality for these individual libraries is included in the SageMaker Training Toolkit and SageMaker Inference Toolkit\.


| Framework | Toolkit Source Code | 
| --- | --- | 
| TensorFlow |  [SageMaker TensorFlow Training](https://github.com/aws/sagemaker-tensorflow-training-toolkit) [SageMaker TensorFlow Serving](https://github.com/aws/sagemaker-tensorflow-serving-container)  | 
| MXNet |  [SageMaker MXNet Training](https://github.com/aws/sagemaker-mxnet-training-toolkit) [SageMaker MXNet Inference](https://github.com/aws/sagemaker-mxnet-inference-toolkit)  | 
| PyTorch |  [SageMaker PyTorch Training](https://github.com/aws/sagemaker-pytorch-training-toolkit) [SageMaker PyTorch Inference](https://github.com/aws/sagemaker-pytorch-inference-toolkit)  | 
| Chainer |  [SageMaker Chainer SageMaker Containers](https://github.com/aws/sagemaker-chainer-container)  | 