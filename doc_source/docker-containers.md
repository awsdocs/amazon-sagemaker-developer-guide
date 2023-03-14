# Using Docker containers with SageMaker<a name="docker-containers"></a>

Amazon SageMaker makes extensive use of *Docker containers* for build and runtime tasks\. SageMaker provides pre\-built Docker images for its built\-in algorithms and the supported deep learning frameworks used for training and inference\. Using containers, you can train machine learning algorithms and deploy models quickly and reliably at any scale\. The topics in this section show how to deploy these containers for your own use cases\. For information about how to bring your own containers for use with Amazon SageMaker Studio, see [Bring your own SageMaker image](studio-byoi.md)\.

**Topics**
+ [Scenarios for Running Scripts, Training Algorithms, or Deploying Models with SageMaker](#container-scenarios)
+ [Docker Container Basics](docker-basics.md)
+ [Use Pre\-built SageMaker Docker images](docker-containers-prebuilt.md)
+ [Adapting your own Docker container to work with SageMaker](docker-containers-adapt-your-own.md)
+ [Create a container with your own algorithms and models](docker-containers-create.md)
+ [Example Notebooks: Use Your Own Algorithm or Model](docker-containers-notebooks.md)
+ [Troubleshooting your Docker containers](docker-containers-troubleshooting.md)

## Scenarios for Running Scripts, Training Algorithms, or Deploying Models with SageMaker<a name="container-scenarios"></a>

Amazon SageMaker always uses Docker containers when running scripts, training algorithms, and deploying models\. Your level of engagement with containers depends on your use case\. 

### Use cases for using pre\-built Docker containers with SageMaker<a name="container-scenarios-use-prebuilt"></a>

Consider the following use cases when using containers with SageMaker:
+ **Pre\-built SageMaker algorithm** – Use the image that comes with the built\-in algorithm\. See [Use Amazon SageMaker Built\-in Algorithms or Pre\-trained Models](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html) for more information\.
+ **Custom model with pre\-built SageMaker container** – If you train or deploy a custom model, but use a framework that has a pre\-built SageMaker container including TensorFlow and PyTorch, choose one of the following options:
  + If you don't need a custom package, and the container already includes all required packages: Use the pre\-built Docker image associated with your framework\. For more information, see [Use Pre\-built SageMaker Docker images](docker-containers-prebuilt.md)\.
  + If you need a custom package installed into one of the pre\-built containers: Confirm that the pre\-built Docker image allows a requirements\.txt file, or extend the pre\-built container based on the following use cases\.

### Use cases for extending a pre\-built Docker container<a name="container-scenarios-extend-prebuilt"></a>

The following are use cases for extending a pre\-built Docker container:
+ **You can't import the dependencies** – Extend the pre\-built Docker image associated with your framework\. See [Extend a Pre\-built Container](prebuilt-containers-extend.md) for more information\.
+ **You can't import the dependencies in the pre\-built container and the pre\-built container supports requirements\.txt** – Add all the required dependencies in requirements\.txt\. The following frameworks support using requirements\.txt\.
  + [TensorFlow](https://sagemaker.readthedocs.io/en/v2.18.0/frameworks/tensorflow/using_tf.html)
  + [Chainer](https://sagemaker.readthedocs.io/en/v2.18.0/frameworks/chainer/using_chainer.html?highlight=requirements.txt)
  + [Sci\-kit learn](https://sagemaker.readthedocs.io/en/stable/frameworks/sklearn/using_sklearn.html?highlight=requirements.txt)
  + [PyTorch](https://sagemaker.readthedocs.io/en/v2.18.0/frameworks/pytorch/using_pytorch.html?highlight=requirements.txt)
  + [Apache MXNet](https://sagemaker.readthedocs.io/en/v2.18.0/frameworks/mxnet/using_mxnet.html?highlight=requirements.txt)

### Use case for building your own container<a name="container-scenarios-byoc"></a>

If you build or train a custom model and require custom framework that does not have a pre\-built image, build a custom container\.

The following decision tree illustrates the information in the previous three lists: **Use cases for using pre\-built Docker containers with SageMaker**; **Use cases for extending a pre\-built Docker container**; **Use case for building your own container**\.

![\[Decision tree for choosing to build a custom container, extend a container, or use a pre-built container.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/your-algorithm-containers-flowchart-diagram.png)

As a customer use case example, to train and deploy a TensorFlow model, refer to the previous sections of **Use cases** to decide which container that you need\. The following are considerations that you can make to choose your container:
+ A TensorFlow model is a custom model\.
+ Because a TensorFlow model is going to be built in the TensorFlow framework, use the TensorFlow pre\-built framework container to train and host the model\.
+ If you require custom packages in either your [ entrypoint](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html#train-a-model-with-tensorflow) script or [inference script, either extend the pre\-built container or use a requirements\.txt file to install dependencies at runtime\.](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/deploying_tensorflow_serving.html#how-to-implement-the-pre-and-or-post-processing-handler-s)

After you determine the type of container that you need, the following information provides details about the previously listed options\.
+ **Use a built\-in SageMaker algorithm or framework**\. For most use cases, you can use the built\-in algorithms and frameworks without worrying about containers\. You can train and deploy these algorithms from the SageMaker console, the AWS Command Line Interface \(AWS CLI\), a Python notebook, or the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. You can do that by specifying the algorithm or framework version when creating your Estimator\. The available built\-in algorithms are itemized and described in the [Use Amazon SageMaker Built\-in Algorithms or Pre\-trained Models](algos.md) topic\. For more information about the available frameworks, see [ML Frameworks and Toolkits](frameworks.md)\. For an example of how to train and deploy a built\-in algorithm using a Jupyter notebook running in a SageMaker notebook instance, see the [Get Started with Amazon SageMaker](gs.md) topic\. 
+ **Use pre\-built SageMaker container images**\. Alternatively, you can use the built\-in algorithms and frameworks using Docker containers\. SageMaker provides containers for its built\-in algorithms and pre\-built Docker images for some of the most common machine learning frameworks, such as Apache MXNet, TensorFlow, PyTorch, and Chainer\. For a full list of the available SageMaker Images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. It also supports machine learning libraries such as scikit\-learn and SparkML\. If you use the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), you can deploy the containers by passing the full container URI to their respective SageMaker SDK `Estimator` class\. For the full list of deep learning frameworks that are currently supported by SageMaker, see [Prebuilt SageMaker Docker Images for Deep Learning](pre-built-containers-frameworks-deep-learning.md)\. For information about the scikit\-learn and SparkML pre\-built container images, see [Prebuilt Amazon SageMaker Docker Images for Scikit\-learn and Spark ML ](pre-built-docker-containers-scikit-learn-spark.md)\. For more information about using frameworks with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), see their respective topics in [Use Machine Learning Frameworks, Python, and R with Amazon SageMaker](frameworks.md)\.
+ **Extend a pre\-built SageMaker container image**\. If you would like to extend a pre\-built SageMaker algorithm or model Docker image, you can modify the SageMaker image to satisfy your needs\. For an example, see [Extending our PyTorch containers](https://sagemaker-examples.readthedocs.io/en/latest/advanced_functionality/pytorch_extending_our_containers/pytorch_extending_our_containers.html)\. 
+ **Adapt an existing container image**: If you would like to adapt a pre\-existing container image to work with SageMaker, you must modify the Docker container to enable either the SageMaker Training or Inference toolkit\. For an example that shows how to build your own containers to train and host an algorithm, see [Bring Your Own R Algorithm](https://github.com/aws/amazon-sagemaker-examples/blob/main/advanced_functionality/scikit_bring_your_own/scikit_bring_your_own.ipynb)\.