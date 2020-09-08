# Prebuilt Amazon SageMaker Docker Images for TensorFlow, MXNet, Chainer, and PyTorch<a name="pre-built-containers-frameworks-deep-learning"></a>

Amazon SageMaker provides prebuilt Docker images that include deep learning framework libraries and other dependencies needed for training and inference\. With the [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk), you can train and deploy models using these popular deep learning frameworks\. For instructions on installing and using the SDK, see [https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk)\. 

The following table provides links to the GitHub repositories that contain the source code and Dockerfiles for each framework and for TensorFlow and MXNet Serving\. The instructions linked are for using the Python SDK to run training algorithms and host models on SageMaker\.


| Framework | Prebuilt Docker Image Source Code | Instructions | 
| --- | --- | --- | 
| TensorFlow |  [SageMaker TensorFlow Containers](https://github.com/aws/sagemaker-tensorflow-container) [SageMaker TensorFlow Serving Containers](https://github.com/aws/sagemaker-tensorflow-serving-container)  |  [Using TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_tf.html)  | 
| MXNet |  [SageMaker MXNet Containers](https://github.com/aws/sagemaker-mxnet-container) [SageMaker MXNet Serving Containers](https://github.com/aws/sagemaker-mxnet-serving-container)  |  [Using MXNet with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_mxnet.html)  | 
| PyTorch |  [SageMaker PyTorch Containers](https://github.com/aws/sagemaker-pytorch-container) [SageMaker PyTorch Serving Containers](https://github.com/aws/sagemaker-pytorch-serving-container)  |  [Using PyTorch with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_pytorch.html)  | 
| Chainer |  [SageMaker Chainer SageMaker Containers](https://github.com/aws/sagemaker-chainer-container)  |  [Using Chainer with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_chainer.html)  | 

If you are not using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) and one of its estimators to retrieve the pre\-built images, you have to retrieve them yourself\. The SageMaker prebuilt Docker images are stored in Amazon Elastic Container Registry \(Amazon ECR\)\. For a complete list of the available pre\-built Docker images, see [Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\.

SageMaker also provides prebuilt Docker images for scikit\-learn and Spark ML\. For information about Docker images that enable using scikit\-learn and Spark ML solutions in SageMaker, see [Prebuilt Amazon SageMaker Docker Images for Scikit\-learn and Spark ML ](pre-built-docker-containers-frameworks.md)\.

You can use prebuilt containers to deploy your custom models or models that have been trained in a framework other than SageMaker\. For an overview of the process of bringing the trained model artifacts into SageMaker and hosting them at an endpoint, see [Bring Your Own Pretrained MXNet or TensorFlow Models into Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/bring-your-own-pre-trained-mxnet-or-tensorflow-models-into-amazon-sagemaker/)\.

You can customize these prebuilt containers or extend them to handle any additional functional requirements for your algorithm or model that the prebuilt SageMaker Docker image doesn't support\. For an example, see [Extending Our PyTorch Containers](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/pytorch_extending_our_containers/pytorch_extending_our_containers.ipynb)\. 