# Prebuilt SageMaker Docker Images for TensorFlow, MXNet, Chainer, and PyTorch<a name="pre-built-containers-frameworks-deep-learning"></a>

SageMaker provides prebuilt Docker images that include deep learning framework libraries and other dependencies needed for training and inference\. For a complete list of the available pre\-built Docker images, see [Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. 

If you are not using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) and one of its estimators to retrieve the pre\-built images, you have to retrieve them yourself\.

## Using the SageMaker Python SDK<a name="pre-built-containers-frameworks-deep-learning-sdk"></a>

With the [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk), you can train and deploy models using these popular deep learning frameworks\. For instructions on installing and using the SDK, see [https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk)\. The following table lists the available frameworks and instructions on how to use them with the [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk):


| Framework | Instructions | 
| --- | --- | 
| TensorFlow |  [Using TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_tf.html)  | 
| MXNet |  [Using MXNet with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_mxnet.html)  | 
| PyTorch |  [Using PyTorch with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_pytorch.html)  | 
| Chainer |  [Using Chainer with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_chainer.html)  | 

## Extending Prebuilt SageMaker Docker Images<a name="pre-built-containers-frameworks-deep-learning-adapt"></a>

You can customize these prebuilt containers or extend them to handle any additional functional requirements for your algorithm or model that the prebuilt SageMaker Docker image doesn't support\. For an example, see [Extending Our PyTorch Containers](https://sagemaker-examples.readthedocs.io/en/latest/advanced_functionality/pytorch_extending_our_containers/pytorch_extending_our_containers.html)\. 

You can also use prebuilt containers to deploy your custom models or models that have been trained in a framework other than SageMaker\. For an overview of the process of bringing the trained model artifacts into SageMaker and hosting them at an endpoint, see [Bring Your Own Pretrained MXNet or TensorFlow Models into Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/bring-your-own-pre-trained-mxnet-or-tensorflow-models-into-amazon-sagemaker/)\.