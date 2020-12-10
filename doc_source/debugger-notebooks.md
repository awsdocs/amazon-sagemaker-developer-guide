# Debugger Example Notebooks<a name="debugger-notebooks"></a>

[SageMaker Debugger example notebooks](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/) are provided in the [aws/amazon\-sagemaker\-examples](https://github.com/aws/amazon-sagemaker-examples) repository\. The Debugger example notebooks walk you through basic to advanced use cases of debugging and profiling training jobs\. 

It is recommended to run the example notebooks on SageMaker Studio or SageMaker Notebook Instance because most of the examples are designed for training jobs in the SageMaker ecosystem, including Amazon EC2, Amazon S3, and Amazon SageMaker Python SDK\. 

To clone the example repository to SageMaker Studio, follow the instructions at [Amazon SageMaker Studio Tour](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-end-to-end.html)\.

To find the examples in a SageMaker Notebook instance, follow the instructions at [SageMaker Notebook Instance Example Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-nbexamples.html)\.

## Debugger Example Notebooks for Analyzing Model Training Progress<a name="debugger-notebooks-debugging"></a>

The following list shows basic Debugger example notebooks introducing adaptability of Debugger to various machine learning models, datasets, and frameworks\.


| Notebook Title | Framework | Model | Dataset | Description | 
| --- | --- | --- | --- | --- | 
|  [Amazon SageMaker Debugger \- Use built\-in rule](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_builtin_rule)  |  TensorFlow  |  Convolutional Neural Network  | MNIST |  Using Amazon SageMaker Debugger built\-in rules for debugging a TensorFlow model  | 
|  [Amazon SageMaker Debugger \- Tensorflow 2\.1](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow2)  |  TensorFlow  |  ResNet50  | Cifar\-10 |  Using Amazon SageMaker Debugger hook configuration and built\-in rules for debugging a model with Tensorflow 2\.1 framework  | 
|  [Visualizing Debugging Tensors of MXNet training](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_plot)  |  MXNet  |  Gluon Convolutional Neural Network  | Fashion MNIST |  Run a training job and configure SageMaker Debugger to store all tensors from this job\. Afterwards we will visualize those tensors in our notebook\.  | 
|  [Enable Spot Training with Amazon SageMaker Debugger](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_spot_training)   | MXNet |  Gluon Convolutional Neural Network  | Fashion MNIST |  Learn how Debugger collects tensor data from a training job on spot instance, and how to use Debugger built\-in rules with managed spot training\.  | 
| [Debugging XGBoost Training Jobs with Amazon SageMaker Debugger Using Rules](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_builtin_rules) | XGBoost |  XGBoost Regression  | Abalone | Learn how to use Debugger hook and built\-in rules for collecting and visualizing tensor data from an XGBoost regression model, such as loss values, features, and SHAP values\. | 

To find advanced visualizations and use cases, see the next topic at [Debugger Advanced Demos and Visualization](debugger-visualization.md)\.