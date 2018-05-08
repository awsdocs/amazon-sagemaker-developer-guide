# Examples: Using Amazon SageMaker with Apache MXNet<a name="sagemaker-examples"></a>

Amazon SageMaker provides the following example notebooks in your Amazon SageMaker notebook instance:
+ **The Apache MXNet `Module` API**— In this example, you use the Amazon SageMaker `sagemaker.mxnet.MXNet` estimator class to train a model\. The custom Apache MXNet training code trains a multilayer perceptron neural network that predicts the number in images of single\-digit handwritten numbers\. It uses images of handwritten numbers as training data\. 
+ **The Apache MXNet Gluon API**—This example uses the Gluon API to do the same thing that the Apache MXNet `Module` API does\.

These examples uses the high\-level Python library provided by Amazon SageMaker\. This library is available on the Amazon SageMaker notebook instance you created as part of Getting Started\. For more information, see [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\. However, if you choose to use your own terminal, you need to download and install the library using one of the following options: 
+ Get it from [https://github\.com/aws/sagemaker\-python\-sdk](https://github.com/aws/sagemaker-python-sdk)\. 
+  Install it using pip:

  ```
  $ pip install sagemaker
  ```

This documentation explains one Apache MXNet example in details\. All of the Apache MXNet example notebooks that Amazon SageMaker provides follow the same pattern\. They differ only in the custom Apache MXNet code they use in model training\. 

**Topics**
+ [Apache MXNet Example 1: Using the Module API](mxnet-example1.md)