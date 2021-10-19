# Debugger Example Notebooks<a name="debugger-notebooks"></a>

[SageMaker Debugger example notebooks](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/) are provided in the [aws/amazon\-sagemaker\-examples](https://github.com/aws/amazon-sagemaker-examples) repository\. The Debugger example notebooks walk you through basic to advanced use cases of debugging and profiling training jobs\. 

We recommend that you run the example notebooks on SageMaker Studio or a SageMaker Notebook instance because most of the examples are designed for training jobs in the SageMaker ecosystem, including Amazon EC2, Amazon S3, and Amazon SageMaker Python SDK\. 

To clone the example repository to SageMaker Studio, follow the instructions at [Amazon SageMaker Studio Tour](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-end-to-end.html)\.

To find the examples in a SageMaker Notebook instance, follow the instructions at [SageMaker Notebook Instance Example Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-nbexamples.html)\.

**Important**  
To use the new Debugger features, you need to upgrade the SageMaker Python SDK and the `SMDebug` client library\. In your iPython kernel, Jupyter Notebook, or JupyterLab environment, run the following code to install the latest versions of the libraries and restart the kernel\.  

```
import sys
import IPython
!{sys.executable} -m pip install -U sagemaker smdebug
IPython.Application.instance().kernel.do_shutdown(True)
```

## Debugger Example Notebooks for Profiling Training Jobs<a name="debugger-notebooks-profiling"></a>

The following list shows Debugger example notebooks introducing Debugger's adaptability to monitor and profile training jobs for various machine learning models, datasets, and frameworks\.


| Notebook Title | Framework | Model | Dataset | Description | 
| --- | --- | --- | --- | --- | 
|  [Amazon SageMaker Debugger Profiling Data Analysis](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/debugger_interactive_analysis_profiling/interactive_analysis_profiling_data.html)  |  TensorFlow  |  Keras ResNet50  | Cifar\-10 |  This notebook provides an introduction to interactive analysis of profiled data captured by SageMaker Debugger\. Explore the full functionality of the `SMDebug` interactive analysis tools\.  | 
|  [Profile machine learning training with Amazon SageMaker Debugger ](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/tensorflow_nlp_sentiment_analysis/sentiment-analysis-tf-distributed-training-bringyourownscript.html)  |  TensorFlow  |  1\-D Convolutional Neural Network  |  IMDB dataset  |  Profile a TensorFlow 1\-D CNN for sentiment analysis of IMDB data that consists of movie reviews labeled as having positive or negative sentiment\. Explore the Studio Debugger insights and Debugger profiling report\.  | 
|  [Profiling TensorFlow ResNet model training with various distributed training settings](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_profiling)  |  TensorFlow  | ResNet50 | Cifar\-10 |  Run TensorFlow training jobs with various distributed training settings, monitor system resource utilization, and profile model performance using Debugger\.  | 
|  [Profiling PyTorch ResNet model training with various distributed training settings](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/pytorch_profiling)   | PyTorch |  ResNet50  | Cifar\-10 |  Run PyTorch training jobs with various distributed training settings, monitor system resource utilization, and profile model performance using Debugger\.  | 

## Debugger Example Notebooks for Analyzing Model Parameters<a name="debugger-notebooks-debugging"></a>

The following list shows Debugger example notebooks introducing Debugger's adaptability to debug training jobs for various machine learning models, datasets, and frameworks\.


| Notebook Title | Framework | Model | Dataset | Description | 
| --- | --- | --- | --- | --- | 
|  [Amazon SageMaker Debugger \- Use built\-in rule](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_builtin_rule)  |  TensorFlow  |  Convolutional Neural Network  | MNIST |  Use the Amazon SageMaker Debugger built\-in rules for debugging a TensorFlow model\.  | 
|  [Amazon SageMaker Debugger \- Tensorflow 2\.1](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow2)  |  TensorFlow  |  ResNet50  | Cifar\-10 |  Use the Amazon SageMaker Debugger hook configuration and built\-in rules for debugging a model with the Tensorflow 2\.1 framework\.  | 
|  [Visualizing Debugging Tensors of MXNet training](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_plot)  |  MXNet  |  Gluon Convolutional Neural Network  | Fashion MNIST |  Run a training job and configure SageMaker Debugger to store all tensors from this job, then visualize those tensors ina notebook\.  | 
|  [Enable Spot Training with Amazon SageMaker Debugger](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_spot_training)   | MXNet |  Gluon Convolutional Neural Network  | Fashion MNIST |  Learn how Debugger collects tensor data from a training job on a spot instance, and how to use the Debugger built\-in rules with managed spot training\.  | 
| [Explain an XGBoost model that predicts an individual’s income with Amazon SageMaker Debugger](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/xgboost_census_explanations/xgboost-census-debugger-rules.html) | XGBoost |  XGBoost Regression  |  [Adult Census dataset](https://archive.ics.uci.edu/ml/datasets/adult)  | Learn how to use the Debugger hook and built\-in rules for collecting and visualizing tensor data from an XGBoost regression model, such as loss values, features, and SHAP values\. | 

To find advanced visualizations of model parameters and use cases, see the next topic at [Debugger Advanced Demos and Visualization](debugger-visualization.md)\.