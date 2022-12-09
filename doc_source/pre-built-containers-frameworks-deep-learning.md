# Prebuilt SageMaker Docker Images for Deep Learning<a name="pre-built-containers-frameworks-deep-learning"></a>

Amazon SageMaker provides prebuilt Docker images that include deep learning frameworks and other dependencies needed for training and inference\. For a complete list of the prebuilt Docker images managed by SageMaker, see [Docker Registry Paths and Example Code](sagemaker-algo-docker-registry-paths.md)\.

## Using the SageMaker Python SDK<a name="pre-built-containers-frameworks-deep-learning-sdk"></a>

With the [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk), you can train and deploy models using these popular deep learning frameworks\. For instructions on installing and using the SDK, see [https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk)\. The following table lists the available frameworks and instructions on how to use them with the [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk):


| Framework | Instructions | 
| --- | --- | 
| TensorFlow |  [Using TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_tf.html)  | 
| MXNet |  [Using MXNet with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_mxnet.html)  | 
| PyTorch |  [Using PyTorch with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_pytorch.html)  | 
| Chainer |  [Using Chainer with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_chainer.html)  | 
| Hugging Face |  [Using Hugging Face with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/index.html)  | 

## Extending Prebuilt SageMaker Docker Images<a name="pre-built-containers-frameworks-deep-learning-adapt"></a>

You can customize these prebuilt containers or extend them to handle any additional functional requirements for your algorithm or model that the prebuilt SageMaker Docker image doesn't support\. For an example, see [Extending Our PyTorch Containers](https://sagemaker-examples.readthedocs.io/en/latest/advanced_functionality/pytorch_extending_our_containers/pytorch_extending_our_containers.html)\. 

You can also use prebuilt containers to deploy your custom models or models that have been trained in a framework other than SageMaker\. For an overview of the process of bringing the trained model artifacts into SageMaker and hosting them at an endpoint, see [Bring Your Own Pretrained MXNet or TensorFlow Models into Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/bring-your-own-pre-trained-mxnet-or-tensorflow-models-into-amazon-sagemaker/)\.