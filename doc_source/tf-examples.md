# Examples: Using Amazon SageMaker with TensorFlow<a name="tf-examples"></a>

**Topics**
+ [TensorFlow Example 1: Using the tf\.estimator](tf-example1.md)

Amazon SageMaker provides the following example notebooks in your Amazon SageMaker notebook instance:
+ **tf\.estimator** \(`iris_dnn_classifier`\)—This introductory example shows how to use the Amazon SageMaker `sagemaker.tensorflow.TensorFlow` estimator class\. It uses this class to create a model for classifying a flower, based on four numerical features\. The custom training code provided for this example uses the DNNClassifier to create a neural network with three hidden layers\.

   
+ **tf\.layers ** \(`abalone_using_layers`\)—This example shows how to use the TensorFlow `layers` module\. It uses the module to create a model that predicts the age of a sea snail, based on seven numerical features\. The custom code creates a neural network by individually specifying its layers\. 

   
+ **tf\.contrib\.keras ** \(`abalone_using_keras`\)—This example shows how to use the TensorFlow Keras library\. It uses the library to create a model that predicts the age of a sea snail\. 

   
+ **distributed TensorFlow ** \(`distributed_mnist`\)—In this example, you explore distributed TensorFlow training in Amazon SageMaker\. You use a convolutional neural network \(CNN\) to classify handwritten numbers and multiple GPU hosts for distributed training\.
+ **ResNet CIFAR\-10 with Tensorboard** \(`resnet_cifar10_with_tensorboard`\)— In this example, you use Tensorboard with SageMaker\. You use a ResNet model to train the CIFAR\-10 dataset and evaluate it using TensorBoard\.

In the examples, only the TensorFlow custom model training code differs\. You interact with Amazon SageMaker the same way in each\. 

The examples use the high\-level Python library provided by Amazon SageMaker\. This library is available in the Amazon SageMaker notebook instance that you created in Getting Started\. If you use your own terminal, download and install the library using one of the following options: 
+ Get it from [https://github\.com/aws/sagemaker\-python\-sdk](https://github.com/aws/sagemaker-python-sdk)\. 
+ Use pip to install it:

  ```
  $ pip install sagemaker 
  ```

The following topic explains one TensorFlow example in detail\. All of the TensorFlow example notebooks that Amazon SageMaker provides follow the same pattern\. They differ only in the custom TensorFlow code that they use for model training\.

There are two ways to use this exercise:
+ Follow the steps to create, deploy, and validate the model\. You create a Jupyter notebook in your Amazon SageMaker notebook instance, and copy code, paste it into the notebook, and run it\.
+ If you're familiar with using notebooks, open and run the example notebook that Amazon SageMaker provides in the notebook instance\.

**Topics**
+ [TensorFlow Example 1: Using the tf\.estimator](tf-example1.md)