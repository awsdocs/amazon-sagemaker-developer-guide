# Amazon SageMaker Debugger<a name="train-debugger"></a>

Amazon SageMaker Debugger provides full visibility into the training of machine learning models by monitoring, recording, and analyzing the tensor data that captures the state of a machine learning training job at each instance in its lifecycle\. It provides a rich set of alerts when detecting errors for the steps of a machine learning training trial, and the ability to perform interactive explorations\. It can automatically detect and alert you to commonly occurring errors such as gradient values getting too large or too small\. When starting a training job that uses Debugger, you configure what tensors to save, where to save the tensors, and the trials to run on your dataset\. The data collected remains in your account to use in subsequent analyses, securing its use for the most privacy\-sensitive applications\. Overall, Debugger can reduce the time to debug the training of models dramatically\.

You can use Amazon SageMaker Debugger from Amazon SageMaker Studio or from Amazon SageMaker notebooks\. Studio makes the inspection of training job issues easier by providing a visual interface for you to use when analyzing your monitoring tensor data\. You can use the Amazon SageMaker Debugger SDK to instrument your code to save tensors if necessary and to build customized rules\. You can then use the Amazon SageMaker Python SDK to configure the debugger to save the required tensors and deploy built\-in or custom rules monitoring these tensors\. The open source *smdebug* Python library at [Amazon SageMaker Debugger](https://github.com/awslabs/sagemaker-debugger/) implements its core debugging functionality\. Debugger also provides the parameters needed to hook up the *smdebug* functionality into the Amazon SageMaker training job APIs that enable it to be deployed on our machine learning platform\.

Using Debugger is a two\-step process: 
+ **Saving tensors \(and scalars\)**: In deep learning algorithms, tensors define the state of the training job at any particular instant in its lifecycle\. Amazon SageMaker Debugger exposes a library that allows you to capture these tensors and save them for analysis\. Debugger is highly customizable and can help provide interoperability by saving performance and other feature\-related metrics at specified frequencies\. 
+ **Analysis**: The analysis of the tensors emitted is captured by the Amazon SageMaker Debugger concept called **rules**\. On a very broad level, a rule is Python code used to detect certain conditions during training\. Some of the conditions that a data scientist training an algorithm may care about are monitoring for gradients getting too large or too small or detecting overfitting\. Debugger comes pre\-packaged with certain rules\. Users can write their own rules using the Debugger APIs\. You can also analyze raw tensor data outside of the rules construct in an Amazon SageMaker notebook, using Debugger's complete set of APIs\. 

## Amazon SageMaker Debugger Sample Notebooks<a name="debugger-sample-notebooks"></a>

Amazon SageMaker Debugger provides sample notebooks that show how to use four learning frameworks to emit tensors in an Amazon SageMaker training job and then how to apply rules over the tensors to monitor the status of these jobs\. It also provides sample notebooks that you can run interactive analysis with using the open source *smdebug* Python library at [Amazon SageMaker Debugger](https://github.com/awslabs/sagemaker-debugger/)\. The following notebooks are listed in the order we recommend you review them\.
+ [Using a built\-in rule with TensorFlow](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_builtin_rule)
+ [Using a custom rule with TensorFlow Kera](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_keras_custom_rule)s
+ [Interactive tensor analysis in notebook with MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_analysis)
+ [Visualizing Debugging Tensors of MXNet training](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_plot)
+ [Real\-time analysis in notebook with MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_realtime_analysis)
+ [Using a built in rule with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_builtin_rules)
+ [Real\-time analysis in notebook with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_realtime_analysis)
+ [Using SageMaker Debugger with Managed Spot Training and MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_spot_training)
+ [Reacting to CloudWatch Events from Rules to take an action based on status with TensorFlow](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_action_on_rule)
+ [Using SageMaker Debugger with a custom PyTorch container](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/pytorch_custom_container)

The links to the Debugger sample notebooks is also available at [Amazon SageMaker Debugger Examples](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger)\.

**Note**  
Although each of these notebooks focuses on a specific framework, the same approach works with all the other frameworks that Amazon SageMaker Debugger supports

## Learning Frameworks and Built\-in Algorithms Supported by Amazon SageMaker Debugger<a name="debugger-supported-fxs-algos"></a>

Amazon SageMaker Debugger supports using the following learning frameworks:
+ Apache MXNet: [Use Apache MXNet with Amazon SageMaker](mxnet.md) 
+ PyTorch: [Use PyTorch with Amazon SageMaker](pytorch.md) 
+ TensorFlow: [Use TensorFlow with Amazon SageMaker](tf.md)
+ XGBoost: [XGBoost Algorithm](xgboost.md)

Amazon SageMaker Debugger supports the following Amazon SageMaker built\-in algorithm:
+ [XGBoost Release 0\.72](xgboost-72.md)—a supervised learning algorithm that is an open\-source implementation of the gradient boosted trees algorithm\.

There are two ways to enable Debugger while training models on Amazon SageMaker: 
+ Use framework containers we provide that do not require any script changes\.
+ Bring your own container and make the script changes needed to enable Debugger 

**Topics**
+ [Zero Script Change](#debugger-zero-script-change)
+ [Bring Your Own Training Container](#debugger-bring-your-own-training-container)

### Zero Script Change<a name="debugger-zero-script-change"></a>

We have provided custom versions of the framework containers we support that enable you to use Amazon SageMaker Debugger with no changes to your training script, by automatically adding Debugger's Hook\. We support the following frameworks and versions for this experience\.


**Supported Frameworks and Versions**  

| Framework | Version | 
| --- | --- | 
| [TensorFlow](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/tensorflow.md) | 1\.15 | 
| [MXNet](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/mxnet.md) | 1\.6 | 
| [PyTorch](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/pytorch.md) | 1\.3 | 
| [XGBoost](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md) | >\+0\.90 \([As built\-in algorithm](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md#use-xgboost-as-a-built-in-algorithm)\) | 

For more information on the deep learning frameworks containers, see [Prebuilt Amazon SageMaker Docker Images for TensorFlow, MXNet, Chainer, and PyTorch](pre-built-containers-frameworks-deep-learning.md)\. 

### Bring Your Own Training Container<a name="debugger-bring-your-own-training-container"></a>

This library *smdebug* itself supports versions other than the ones listed above\. If you want to use SageMaker Debugger with a version different from the above, you will have to orchestrate your training script with a few lines\. Before we discuss how these changes look like, let us take a look at the versions supported\.


**Supported Frameworks and Versions**  

| Framework | Version | 
| --- | --- | 
| [TensorFlow](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/tensorflow.md) | 1\.13, 1\.14\. 1\.15 | 
| Keras \(with TensorFlow backend\) | 2\.3 | 
| [MXNet](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/mxnet.md) | 1\.4, 1\.4, 1\.6 | 
| [PyTorch](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/pytorch.md) | 1\.2, 1\.3 | 
| [XGBoost](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md) | \([As a framework](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md#use-xgboost-as-a-framework)\) | 

To set up Debugger with your script on your container:
+ Ensure that you are using Python 3\+ runtime as smdebug only supports Python 3 or higher\.
+ Install *smdebug* binary through `pip install smdebug`
+ Make some minimal modifications to your training script to add the `Hook` for SageMaker Debugger\. Refer to the framework pages linked in the table for instructions\.

## Amazon CloudWatch Metrics for Amazon SageMaker Debugger<a name="debugger-cloudwatch"></a>

Amazon CloudWatch collects model training metrics so you can monitor training jobs\. See the **Processing Job, Training Job, Batch Transform Job, and Endpoint Instance Metrics** sections in the in [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md) topic\.

**Topics**
+ [Amazon SageMaker Debugger Sample Notebooks](#debugger-sample-notebooks)
+ [Learning Frameworks and Built\-in Algorithms Supported by Amazon SageMaker Debugger](#debugger-supported-fxs-algos)
+ [Amazon CloudWatch Metrics for Amazon SageMaker Debugger](#debugger-cloudwatch)
+ [How Debugger Works](debugger-how-it-works.md)
+ [Save Tensor Data for Debugger](debugger-data.md)
+ [Prebuilt Amazon SageMaker Docker Images for Rules](debugger-docker-images-rules.md)
+ [Built\-in Rules Provided by Amazon SageMaker Debugger](debugger-built-in-rules.md)
+ [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)
+ [Programming Model for Debugger](debugger-programming-model.md)
+ [How to Use Custom Rules](debugger-custom-rules.md)
+ [Amazon SageMaker Studio Visualizations of Model Analysis Results](debugger-visualization.md)
+ [Amazon SageMaker Debugger Reference Documentation](debugger-reference.md)