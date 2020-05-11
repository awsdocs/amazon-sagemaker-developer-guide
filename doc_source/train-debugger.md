# Amazon SageMaker Debugger<a name="train-debugger"></a>

Amazon SageMaker Debugger provides full visibility into model training by monitoring, recording, and analyzing the tensor data that captures the state of a training job\. Debugger provides alerts that are automatically triggered when it detects common errors during model training, such as when gradient values get too low or too high, and allows you to interactively examine them\. Debugger can dramatically reduce the time needed to debug model training\. 

To use Amazon SageMaker Debugger, you must configure your training job to use it when you create the job\. You configure which tensors to save, where to save them, and the trials to run on your dataset\. The data that Debugger collects remains in your account to use in subsequent analyses where it is secured for use with the most privacy\-sensitive applications\. 

You can use Amazon SageMaker Debugger in Amazon SageMaker Studio or Amazon SageMaker notebooks\. Using Studio makes inspecting training job issues easier because it provides a visual interface for analyzing your tensor data\. To save tensors and, if necessary, to build customized rules to analyze them, use the Amazon SageMaker Debugger SDK\. You can then use the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to configure Debugger to save the required tensors and to deploy built\-in or custom rules for monitoring them\. The open source smdebug Python library at [Amazon SageMaker Debugger](https://github.com/awslabs/sagemaker-debugger/) implements the core debugging functionality\. Debugger also provides the parameters needed to run *smdebug* with the Amazon SageMaker training job APIs that enable deploying Debugger on our Amazon SageMaker platform\.

Using Amazon SageMaker Debugger is a two\-step process: 
+ **Save tensors**: In deep learning algorithms, *tensors* define the state of the training job at any particular point in its lifecycle\. Amazon SageMaker Debugger exposes a Python library, smdebug, that allows you to capture these tensors and save them for analysis\. Debugger is highly customizable and can help provide interoperability by saving performance and other feature\-related metrics at specified frequencies\. 
+ **Analyze data**: To analyze emitted tensors, you use rules\. A *rule* is Python code that detects certain conditions during training, for example, gradients growing too large or too small or overfitting\.\. Debugger provides common rules\. To create your own rules, you can usie the Debugger APIs\. You can also analyze raw tensor data without using rules in an Amazon SageMaker notebook, using Debugger APIs\. 

## Amazon SageMaker Debugger Sample Notebooks<a name="debugger-sample-notebooks"></a>

Amazon SageMaker Debugger provides sample notebooks that show how to use four learning frameworks to emit tensors in an Amazon SageMaker training job and how to apply rules to the tensors to monitor the status of the jobs\. There are also sample notebooks that you can run interactive analysis with using the open source *smdebug* Python library at [Amazon SageMaker Debugger](https://github.com/awslabs/sagemaker-debugger/)\. We recommend using the notebooks in the following order:
+ [Using a built\-in rule with TensorFlow](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_builtin_rule)
+ [Using a custom rule with TensorFlow Keras](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_keras_custom_rule)
+ [Interactive tensor analysis in notebook with MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_analysis)
+ [Visualizing Debugging Tensors of MXNet training](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_plot)
+ [Real\-time analysis in notebook with MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_realtime_analysis)
+ [Using a built in rule with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_builtin_rules)
+ [Real\-time analysis in notebook with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_realtime_analysis)
+ [Using SageMaker Debugger with Managed Spot Training and MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_spot_training)
+ [Reacting to CloudWatch Events from Rules to take an action based on status with TensorFlow](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_action_on_rule)
+ [Using SageMaker Debugger with a custom PyTorch container](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/pytorch_custom_container)

Links to the Amazon SageMaker Debugger sample notebooks are also available at [Amazon SageMaker Debugger Examples](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger)\.

**Note**  
Although each of these notebooks focuses on a specific framework, the approach shown in each works with all of the other frameworks that Amazon SageMaker Debugger supports\.

## Learning Frameworks and Built\-in Algorithms Supported by Amazon SageMaker Debugger<a name="debugger-supported-fxs-algos"></a>

You can use Amazon SageMaker Debugger with the following learning frameworks:
+ Apache MXNet: [Use Apache MXNet with Amazon SageMaker](mxnet.md) 
+ PyTorch: [Use PyTorch with Amazon SageMaker](pytorch.md) 
+ TensorFlow: [Use TensorFlow with Amazon SageMaker](tf.md)
+ XGBoost: [XGBoost Algorithm](xgboost.md)

Amazon SageMaker Debugger supports the following Amazon SageMaker built\-in algorithm:
+ [XGBoost Algorithm](xgboost.md)—XGBoost is a supervised learning algorithm that is an open\-source implementation of the gradient boosted trees algorithm\.

To enable Amazon SageMaker Debugger, you use a container\. There are two ways to enable Debugger for model training 
+ Use one of the framework containers that we provide\. These containers don't require any additional script changes\.
+ Use your own container and make the script changes needed to enable it to use Debugger 

**Topics**
+ [Use a Provided Pre\-built Framework Container to Enable Amazon SageMaker Debugger](#debugger-zero-script-change)
+ [Use Your Own Training Container to Enable Amazon SageMaker Debugger](#debugger-bring-your-own-training-container)

### Use a Provided Pre\-built Framework Container to Enable Amazon SageMaker Debugger<a name="debugger-zero-script-change"></a>

To enable Amazon SageMaker Debugger without making changes to your training script, use the container for your framework listed in the following table\. Each of these containers automatically adds the Debugger hook, a callback that Debugger use to save the values of requested tensors throughout the training process\.


**Supported Frameworks and Versions**  

| Framework | Framework Version | 
| --- | --- | 
| [TensorFlow](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/tensorflow.md) | 1\.15 | 
| [MXNet](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/mxnet.md) | 1\.6 | 
| [PyTorch](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/pytorch.md) | 1\.3 | 
| [XGBoost](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md) | >=0\.90\-2 \([As built\-in algorithm](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md#use-xgboost-as-a-built-in-algorithm)\) | 

Refer to [zero script change containers](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/sagemaker.md#zero-script-change) for the most recent framework information, and the [Amazon SageMaker Debugger configuration guide](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/sagemaker.md#configuring-sagemaker-debugger)\.

### Use Your Own Training Container to Enable Amazon SageMaker Debugger<a name="debugger-bring-your-own-training-container"></a>

The smdebug library is an open source Python Library that is used to configure built\-in rules or to define custom rules used to analyze the tensor data from training jobs\. It supports versions of the deep learning frameworks other than those listed in the [Use a Provided Pre\-built Framework Container to Enable Amazon SageMaker Debugger](#debugger-zero-script-change) section\. The following table lists the framework versions that are supported by smdebug\.


**Supported Frameworks and Versions**  

| Framework | Version | 
| --- | --- | 
| [TensorFlow](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/tensorflow.md) | 1\.14\. 1\.15, 2\.0\.1, 2\.1\.0 | 
|  Keras \(with TensorFlow backend\)  | 2\.3 | 
| [MXNet](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/mxnet.md) | 1\.4, 1\.5, 1\.6 | 
| [PyTorch](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/pytorch.md) | 1\.2, 1\.3 | 
| [XGBoost](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md) | \([As a framework](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md#use-xgboost-as-a-framework)\) | 

To use Amazon SageMaker Debugger with one of these framework versions, you need to add a few lines to your training script\. You also need to have the Python 3\.0 or later version runtime installed because smdebug supports only versions 3\.0 and later\.

To set up Amazon SageMaker Debugger with your script on your container:

1. Ensure that you are using the Python 3\.0 or later runtime\.

1. Use the `pip install smdebug` command to install the smdebug binary\.

1. Modiy your training script to add the `Hook` needed to wire up Debugger\. For instructions, see the information provided for your framework by choosing the appropriate link in the table\.

## Amazon CloudWatch Metrics for Amazon SageMaker Debugger<a name="debugger-cloudwatch"></a>

Amazon CloudWatch collects model training metrics so you can monitor training jobs\. For more information, see [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md)\.