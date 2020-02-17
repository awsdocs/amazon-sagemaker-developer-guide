# Prebuilt Amazon SageMaker Docker Images for TensorFlow, MXNet, Chainer, and PyTorch<a name="pre-built-containers-frameworks-deep-learning"></a>

Amazon SageMaker provides prebuilt Docker images that include deep learning framework libraries and other dependencies needed for training and inference\. With the [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk), you can train and deploy models using one of these popular deep learning frameworks\. For instructions on installing and using the SDK, see [Amazon SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk)\. 

The following table provides links to the GitHub repositories that contain the source code and Dockerfiles for each framework and for TensorFlow and MXNet Serving\. The instructions linked are for using the Python SDK estimators to run your own training algorithms on Amazon SageMaker and your own models on Amazon SageMaker hosting\.


| Framework | Prebuilt Docker Image Source Code | Instructions | 
| --- | --- | --- | 
| TensorFlow |  [Amazon SageMaker TensorFlow Containers](https://github.com/aws/sagemaker-tensorflow-container) [Amazon SageMaker TensorFlow Serving Container](https://github.com/aws/sagemaker-tensorflow-serving-container)  |  [Using TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_tf.html)  | 
| MXNet |  [Amazon SageMaker MXNet Containers](https://github.com/aws/sagemaker-mxnet-container) [Amazon SageMaker MXNet Serving Container](https://github.com/aws/sagemaker-mxnet-serving-container)  |  [Using MXNet with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_mxnet.html)  | 
| Chainer |  [Amazon SageMaker Chainer SageMaker Containers](https://github.com/aws/sagemaker-chainer-container)  |  [Chainer SageMaker Estimators and Models](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/chainer/README.rst)  | 
| PyTorch |  [Amazon SageMaker PyTorch Containers](https://github.com/aws/sagemaker-pytorch-container)  |  [SageMaker PyTorch Estimators and Models](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/pytorch/README.rst)  | 

If you are not using the Amazon SageMaker Python SDK and one of its estimators to manage the container, you have to retrieve the relevant pre\-built container\. The Amazon SageMaker prebuilt Docker images are stored in Amazon Elastic Container Registry \(Amazon ECR\)\. To pull an image from an Amazon ECR repo or to push an image to an Amazon ECR repo, use fullname registry address of the image\. Amazon SageMaker uses the following URL patterns for the Deep Learning container images:
+ URL pattern for Python 3 images for training with TensorFlow\-1\.13 and later or for training or serving with MXNet\-1\.4\.1 and later \(except for the Elastic Inference containers `sagemaker-tensorflow-eia` and `sagemaker-mxnet-serving-eia`\):
  + `763104351884.dkr.ecr.<region>.amazonaws.com/<ECR repo name>:<framework version>-<processing unit type>-<python version>` 

    **Example** of an Amazon ECR URI for the MXNet 1\.4\.1 training image:

     `763104351884.dkr.ecr.us-east-1.amazonaws.com/mxnet-training:1.4.1-gpu-py3`\.
+ URL pattern for Python 3 images for serving with TensorFlow\-1\.13 and later:
  + `763104351884.dkr.ecr.<region>.amazonaws.com/tensorflow-inference:<framework version>-<processing unit type>`

    **Example** of an Amazon ECR URI for the TensorFlow 1\.13 serving image:

     `763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference:1.13-gpu`\.
+ URL pattern for images with Chainer, PyTorch, Python 2 versions of TensorFlow or MXNet, and for the Elastic Inference containers `sagemaker-tensorflow-eia` and `sagemaker-mxnet-serving-eia`:
  + `520713654638.dkr.ecr.<region>.amazonaws.com/<ECR repo name>:<framework version>-<processing unit type>-<python version>`

    **Example** of an Amazon ECR URI for the Chainer 4\.1\.0 training image:

     `520713654638.dkr.ecr.us-east-1.amazonaws.com/sagemaker-chainer:4.1.0-gpu-py2`\.

For the supported values for the components in the URL addresses, see the following table\.


| URL Component | Description | Supported Values | 
| --- | --- | --- | 
| <ECR repo name> |  Specifies the public repository owned by Amazon SageMaker in the Amazon ECR\.  |  Python 3 containers for TensorFlow\-1\.13 and later and MXNet\-1\.4\.1 and later: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-containers-frameworks-deep-learning.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-containers-frameworks-deep-learning.html)  | 
| <framework version> |  Specifies the framework and links to documentation for the estimators for each of the frameworks that explains how to specify the supported versions\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-containers-frameworks-deep-learning.html)  | 
| <processing unit type> |  Specifies whether to use a GPU or CPU for training or hosting\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-containers-frameworks-deep-learning.html)  | 
| <python version> |  Specifies the version of Python used\. \(Optional if you are using the `tensorflow-inference` container for serving\.\)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-containers-frameworks-deep-learning.html)  | 

Amazon SageMaker also provides prebuilt Docker images for scikit\-learn and Spark ML\. For information about Docker images that enable using scikit\-learn and Spark ML solutions in Amazon SageMaker, see [Prebuilt Amazon SageMaker Docker Images for Scikit\-learn and Spark ML ](pre-built-docker-containers-frameworks.md)\.

You can use prebuilt containers to deploy your custom models or models that you have purchased on AWS Marketplace that have been trained in a framework other than Amazon SageMaker\. For an overview of the process of bringing the trained model artifacts into Amazon SageMaker and hosting them at an endpoint, see [Bring Your Own Pretrained MXNet or TensorFlow Models into Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/bring-your-own-pre-trained-mxnet-or-tensorflow-models-into-amazon-sagemaker/)\.

You can customize these prebuilt containers or extend them to handle any additional functional requirements for your algorithm or model that the prebuilt Amazon SageMaker Docker image doesn't support\. For an example, see [Extending Our PyTorch Containers](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/pytorch_extending_our_containers/pytorch_extending_our_containers.ipynb)\. 
