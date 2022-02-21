# Example Notebooks: Use Your Own Algorithm or Model<a name="docker-containers-notebooks"></a>

The following Jupyter notebooks show how to use your own algorithms or pretrained models from an Amazon SageMaker notebook instance\. For links to the GitHub repositories with the prebuilt Dockerfiles for the TensorFlow, MXNet, Chainer, and PyTorch frameworks and instructions on using the AWS SDK for Python \(Boto3\) estimators to run your own training algorithms on SageMaker Learner and your own models on SageMaker hosting, see [Prebuilt SageMaker Docker Images for Deep Learning](pre-built-containers-frameworks-deep-learning.md)

## Setup<a name="docker-containers-notebooks-setup"></a>

1. Create a SageMaker notebook instance\. For instructions on how to create and access Jupyter notebook instances, see [Use Amazon SageMaker Notebook Instances](nbi.md)\.

1. Open the notebook instance you created\.

1. Choose the **SageMaker Examples** tab for a list of all SageMaker example notebooks\.

1. Open the sample notebooks from the **Advanced Functionality** section in your notebook instance or from GitHub using the provided links\. To open a notebook, choose its **Use** tab, then choose **Create copy**\.

## Host Models Trained in Scikit\-learn<a name="docker-containers-notebooks-scikit"></a>

To learn how to host models trained in Scikit\-learn for making predictions in SageMaker by injecting them into first\-party k\-means and XGBoost containers, see the following sample notebooks\.
+ [kmeans\_bring\_your\_own\_model](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/kmeans_bring_your_own_model)
+ [xgboost\_bring\_your\_own\_model](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/xgboost_bring_your_own_model)

## Package TensorFlow and Scikit\-learn Models for Use in SageMaker<a name="docker-containers-notebooks-package"></a>

To learn how to package algorithms that you have developed in TensorFlow and scikit\-learn frameworks for training and deployment in the SageMaker environment, see the following notebooks\. They show you how to build, register, and deploy your own Docker containers using Dockerfiles\.
+ [tensorflow\_bring\_your\_own](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/tensorflow_bring_your_own)
+ [scikit\_bring\_your\_own](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/scikit_bring_your_own)

## Train and Deploy a Neural Network on SageMaker<a name="docker-containers-notebooks-neural"></a>

To learn how to train a neural network locally using MXNet or TensorFlow, and then create an endpoint from the trained model and deploy it on SageMaker, see the following notebooks\. The MXNet model is trained to recognize handwritten numbers from the MNIST dataset\. The TensorFlow model is trained to classify irises\.
+ [mxnet\_mnist\_byom](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/mxnet_mnist_byom)
+ [tensorflow\_iris\_byom](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/tensorflow_iris_byom)

## Training Using Pipe Mode<a name="docker-containers-notebooks-pipe"></a>

To learn how to use a Dockerfile to build a container that calls the `train.py script` and uses pipe mode to custom train an algorithm, see the following notebook\. In pipe mode, the input data is transferred to the algorithm while it is training\. This can decrease training time compared to using file mode\. 
+ [pipe\_bring\_your\_own](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/pipe_bring_your_own)

## Bring Your Own R Model<a name="docker-containers-notebooks-r"></a>

To learn how to use an R container to train and host a model with the R kernel installed in a notebook , see the following notebook\. To take advantage of the AWS SDK for Python \(Boto3\), we use Python within the notebook\. You can achieve the same results in R by invoking command line arguments\.
+ [r\_bring\_your\_own](https://github.com/aws/amazon-sagemaker-examples/blob/master/r_examples/r_byo_r_algo_hpo/tune_r_bring_your_own.ipynb)

## Extend a Prebuilt PyTorch Container Image<a name="docker-containers-notebooks-extend"></a>

To learn how to extend a prebuilt SageMaker PyTorch container image when you have additional functional requirements for your algorithm or model that the prebuilt Docker image doesn't support, see the following notebook\.
+ [pytorch\_extending\_our\_containers ](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/pytorch_extending_our_containers)

## Train and Debug Training Jobs on a Custom Container<a name="docker-containers-notebooks-debugger"></a>

To learn how to train and debug training jobs using SageMaker Debugger, see the following notebook\. A training script provided through this example uses the TensorFlow Keras ResNet 50 model and the CIFAR10 dataset\. A Docker custom container is built with the training script and pushed to Amazon ECR\. While the training job is running, Debugger collects tensor outputs and identifies debugging problems\. With `smdebug` client library tools, you can set a `smdebug` trial object that calls the training job and debugging information, check the training and Debugger rule status, and retrieve tensors saved in an Amazon S3 bucket to analyze training issues\.
+ [build\_your\_own\_container\_with\_debugger](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/build_your_own_container_with_debugger/debugger_byoc.html)
