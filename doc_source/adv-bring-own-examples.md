# Example Notebooks: Use Your Own Algorithm or Model<a name="adv-bring-own-examples"></a>

The following sample notebooks show how to use your own algorithms or pretrained models from an Amazon SageMaker notebook instance\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab for a list of all SageMaker example notebooks\. You can open the sample notebooks from the **Advanced Functionality** section in your notebook instance or in GitHub at the provided links\. To open a notebook, choose its **Use** tab, then choose **Create copy**\.

For instructions on how to create and access Jupyter notebook instances, see [Use Amazon SageMaker Notebook Instances](nbi.md)

To learn how to **host models trained in scikit\-learn** for making predictions in SageMaker by injecting them first\-party k\-means and XGBoost containers, see the following sample notebooks\.
+ kmeans\_bring\_your\_own\_model \- [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/advanced\_functionality/kmeans\_bring\_your\_own\_model](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/kmeans_bring_your_own_model)
+ xgboost\_bring\_your\_own\_model \- [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/advanced\_functionality/xgboost\_bring\_your\_own\_model](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/xgboost_bring_your_own_model)

To learn how to **package algorithms that you have developed in TensorFlow and scikit\-learn frameworks for training and deployment in the SageMaker** environment, see the following notebooks\. They show you how to build, register, and deploy you own Docker containers using Dockerfiles\.
+ tensorflow\_bring\_your\_own \- [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/advanced\_functionality/tensorflow\_bring\_your\_own](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/tensorflow_bring_your_own)
+ scikit\_bring\_your\_own \- [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/advanced\_functionality/scikit\_bring\_your\_own](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/scikit_bring_your_own)

To learn how to **train a neural network locally using MXNet or TensorFlow**, and then create an endpoint from the trained model and **deploy it on SageMaker**, see the following notebooks\. The MXNet model is trained to recognize handwritten numbers from the MNIST dataset\. The TensorFlow model is trained to classify irises\.
+ mxnet\_mnist\_byom \- [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/advanced\_functionality/mxnet\_mnist\_byom](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/mxnet_mnist_byom)
+ tensorflow\_iris\_byom \- [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/advanced\_functionality/tensorflow\_iris\_byom](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/tensorflow_iris_byom)

To learn how to **use a Dockerfile to build a container that calls the `train.py script`** and uses pipe mode to custom train an algorithm, see the following notebook\. In pipe mode, the input data is transferred to the algorithm while it is training\. This can decrease training time compared to using file\-mode\. 
+ pipe\_bring\_your\_own \- [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/advanced\_functionality/pipe\_bring\_your\_own](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/pipe_bring_your_own)

To learn how to** use an R container to train and host a model** with the R kernel installed in a notebook , see the following notebook\. To take advantage of the AWS SDK for Python \(Boto3\), we use Python within the notebook\. You can achieve the same results completely in R by invoking command line arguments\.
+ r\_bring\_your\_own \- [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/advanced\_functionality/r\_bring\_your\_own](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/r_bring_your_own)

To learn how to **extend a prebuilt SageMaker PyTorch container image** when you have additional functional requirements for your algorithm or model that the pre\-built Docker image doesn't support, see the following notebook\.
+ pytorch\_extending\_our\_containers \- [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/advanced\_functionality/pytorch\_extending\_our\_containers](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/pytorch_extending_our_containers)

For links to the GitHub repositories with the prebuilt Dockerfiles for the TensorFlow, MXNet, Chainer, and PyTorch frameworks and instructions on use the AWS SDK for Python \(Boto3\) estimators to run your own training algorithms on SageMaker Learner and your own models on SageMaker hosting, see [Prebuilt Amazon SageMaker Docker Images for TensorFlow, MXNet, Chainer, and PyTorch](pre-built-containers-frameworks-deep-learning.md)