# Pre\-built Amazon SageMaker Docker Images for Tensorflow, MXNet, Chainer,and PyTorch<a name="pre-built-docker-containers-frameworks-deep-learning"></a>

The *pre\-built Dockerfiles* provided by Amazon SageMaker contain the Dockerfiles which install the deep learning frameworks and the dependencies they need to build SageMaker\-compatible Docker images using the Amazon SageMaker Python SDK\. With the SDK, you can train and deploy models using one of the popular deep learning frameworks\. For instructions on installing and using the SDK, see [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk)\. The following table contains links to the github repositories with the pre\-built Dockerfiles for each framework and to instructions that show how use the Python SDK estimators to run your own training algorithms on Amazon SageMaker Learner and your own models on Amazon SageMaker Hosting\.


| Framework | Pre\-built Dockerfiles | Instructions | 
| --- | --- | --- | 
| TensorFlow |  [SageMaker TensorFlow Containers](https://github.com/aws/sagemaker-tensorflow-container)  |  [Using TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_tf.html)  | 
| MXNet |  [MXNet SageMaker Containers](https://github.com/aws/sagemaker-mxnet-container)  |  [Using MXNet with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_mxnet.html)  | 
| Chainer |  [Chainer SageMaker Containers](https://github.com/aws/sagemaker-chainer-container)  |  [Chainer SageMaker Estimators and Models](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/chainer/README.rst)  | 
| PyTorch |  [SageMaker PyTorch Containers](https://github.com/aws/sagemaker-python-sdk#pytorch-sagemaker-estimators )  |  [SageMaker PyTorch Estimators and Models](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/pytorch/README.rst)  | 

The Amazon SageMaker pre\-built Docker images are stored in the Amazon ECR\. They can be pushed or pulled using their fullname registry addresses\. Here are the Docker Image URL Patterns used by Amazon SageMaker:
+ `520713654638.dkr.ecr.<region>.amazonaws.com/<ECR repo name>:<framework version>-<processing unit type>-<python version>`
+ `520713654638.dkr.ecr.<region>.amazonaws.com/<ECR repo name>:<framework version>-<processing unit type>`

Here are the supported values for the components in the URL addresses\.


| URL Component | Description | Supported Values | 
| --- | --- | --- | 
| <ECR repo name> |  Specifies the public repository owned by Amazon SageMaker in the Amazon Elastic Container Registry \(Amazon ECR\)\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-docker-containers-frameworks-deep-learning.html)  | 
| <framework version> |  Specifies the framework and links to documentation of the estimators for each of the frameworks that contains details of how to specify the specific versions supported\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-docker-containers-frameworks-deep-learning.html)  | 
| <processing unit type> |  Specifies whether GPU or CPU is used for training or hosting\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-docker-containers-frameworks-deep-learning.html)  | 
| <python version> |  Specifies the version of Python used\. \(This component is optional if using sagemaker\-tensorflow\-serving\.\)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/pre-built-docker-containers-frameworks-deep-learning.html)  | 

Amazon SageMaker also provides pre\-built Docker images for scikit\-learn and Spark ML\. For information on Docker images that enable scikit\-learn and Spark ML solutions to be used in Amazon SageMaker, see [Pre\-built Amazon SageMaker Docker Images for Scikit\-learn and Spark ML ](pre-built-docker-containers-frameworks.md)\.

Pre\-build containers can be used to deploy your models or models that you have purchased on AWS Marketplace with Amazon SageMaker that have been trained in a framework outside of Amazon SageMaker\. For an overview of the process of bringing the trained model artifacts into Amazon SageMaker and hosting them at an endpoint, see [Bring your own pre\-trained MXNet or TensorFlow models into Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/bring-your-own-pre-trained-mxnet-or-tensorflow-models-into-amazon-sagemaker/)\.

These pre\-build containers can also be customized or extended to handle any additional functional requirements for your algorithm or model that the pre\-built Amazon SageMaker Docker image doesn't support\. For an example, see [Extending our PyTorch containers](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/pytorch_extending_our_containers/pytorch_extending_our_containers.ipynb)\. For an example involving transfer learning, see [Transfer learning for custom labels using a TensorFlow container](https://aws.amazon.com/blogs/machine-learning/transfer-learning-for-custom-labels-using-a-tensorflow-container-and-bring-your-own-algorithm-in-amazon-sagemaker/)\.