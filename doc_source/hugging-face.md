# Use Hugging Face with Amazon SageMaker<a name="hugging-face"></a>

Amazon SageMaker enables customers to train, fine\-tune, and run inference using Hugging Face models for Natural Language Processing \(NLP\) on SageMaker\. You can use any of the thousands of models available in Hugging Face and fine\-tune them for your specific use case with additional training\. With SageMaker, you can use standard training or take advantage of [SageMaker Distributed Data and Model Parallel training](https://docs.aws.amazon.com/sagemaker/latest/dg/distributed-training.html)\. You can also debug your training jobs using [Amazon SageMaker Debugger](https://docs.aws.amazon.com/sagemaker/latest/dg/train-debugger.html)\. As with other SageMaker training jobs using custom code, you can capture your own metrics by passing a metrics definition to the SageMaker Python SDK as shown in [Defining Training Metrics \(SageMaker Python SDK\) ](https://docs.aws.amazon.com/sagemaker/latest/dg/training-metrics.html#define-train-metrics-sdk) \. The captured metrics are then accessible via [CloudWatch](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html) and as a Pandas `DataFrame` via the [TrainingJobAnalytics](https://sagemaker.readthedocs.io/en/stable/api/training/analytics.html#sagemaker.analytics.TrainingJobAnalytics) method\. Once your model is trained and fine\-tuned, you can use it like any other model to run inference jobs\.

This functionality is available through the development of a Hugging Face [Deep Learning Container](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/what-is-dlc.html)\. These containers include Hugging Face Transformers, Tokenizers and the Datasets library, which allows you to use these resources for your training jobs\. For a list of the available DLC images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\.

To use the Hugging Face Deep Learning Container with the SageMaker Python SDK, see the [Hugging Face SageMaker Estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/index.html)\. With the Hugging Face Estimator, you can use the Hugging Face resources as you would any other SageMaker estimator\.

For more information on Hugging Face and the models available in it, see the [Hugging Face documentation](https://huggingface.co/)\. 

## How to Use the Hugging Face Estimator<a name="hugging-face-using"></a>

You can implement the Hugging Face Estimator for training jobs using the SageMaker Python SDK\. The SageMaker Python SDK is an open source library for training and deploying machine learning models on SageMaker\. For more information on the Hugging Face Estimator, see the [SageMaker Python SDK documentation\.](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/index.html)

With the SageMaker Python SDK, you can run training jobs using the Hugging Face Estimator in the following environments: 
+ [SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html): Amazon SageMaker Studio is the first fully integrated development environment \(IDE\) for machine learning \(ML\)\. SageMaker Studio provides a single, web\-based visual interface where you can perform all ML development steps required to prepare, build, train and tune, deploy and manage models\. For information on using Jupyter Notebooks in Studio, see [Use Amazon SageMaker Studio Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks.html)\.
+ [SageMaker Notebook Instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html): An Amazon SageMaker notebook instance is a machine learning \(ML\) compute instance running the Jupyter Notebook App\. This app lets you run Jupyter Notebooks in your notebook instance to prepare and process data, write code to train models, deploy models to SageMaker hosting, and test or validate your models without SageMaker Studio features like Debugger, Model Monitoring, and a web\-based IDE\.
+ Locally: If you have connectivity to AWS and have appropriate SageMaker permissions, you can use the SageMaker Python SDK locally to launch remote training and inference jobs for Hugging Face in SageMaker on AWS\. 

 **What do you want to do?** 

The following Jupyter Notebooks illustrate how to use the Hugging Face Estimator with SageMaker in various use cases\.

I want to train a text classification model using Hugging Face in SageMaker with PyTorch\.  
For a sample Jupyter Notebook, see the [PyTorch Getting Started Demo](https://github.com/huggingface/notebooks/blob/master/sagemaker/01_getting_started_pytorch/sagemaker-notebook.ipynb)\.

I want to train a text classification model using Hugging Face in SageMaker with TensorFlow\.  
For a sample Jupyter Notebook, see the [TensorFlow Getting Started example](https://github.com/huggingface/notebooks/blob/master/sagemaker/02_getting_started_tensorflow/sagemaker-notebook.ipynb)\.

I want to run distributed training with data parallelism using Hugging Face and SageMaker Distributed\.  
For a sample Jupyter Notebook, see the [Distributed Training example](https://github.com/huggingface/notebooks/blob/master/sagemaker/03_distributed_training_data_parallelism/sagemaker-notebook.ipynb)\.

I want to run distributed training with model parallelism using Hugging Face and SageMaker Distributed\.  
For a sample Jupyter Notebook, see the [Model Parallelism example](https://github.com/huggingface/notebooks/blob/master/sagemaker/04_distributed_training_model_parallelism/sagemaker-notebook.ipynb)\.

I want to use a spot instance to train a model using Hugging Face in SageMaker\.  
For a sample Jupyter Notebook, see the [Spot Instances example](https://github.com/huggingface/notebooks/blob/master/sagemaker/05_spot_instances/sagemaker-notebook.ipynb)\.

I want to capture custom metrics and use SageMaker Checkpointing when training a text classification model using Hugging Face in SageMaker\.  
For a sample Jupyter Notebook, see the [Training with Custom Metrics example](https://github.com/huggingface/notebooks/blob/master/sagemaker/06_sagemaker_metrics/sagemaker-notebook.ipynb)\.

I want to train a distributed question\-answering TensorFlow model using Hugging Face in SageMaker\.  
For a sample Jupyter Notebook, see the [Distributed TensorFlow Training example](https://github.com/huggingface/notebooks/blob/master/sagemaker/07_tensorflow_distributed_training_data_parallelism/sagemaker-notebook.ipynb)\.