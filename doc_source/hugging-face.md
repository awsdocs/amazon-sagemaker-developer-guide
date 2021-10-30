# Use Hugging Face with Amazon SageMaker<a name="hugging-face"></a>

Amazon SageMaker enables customers to train, fine\-tune, and run inference using Hugging Face models for Natural Language Processing \(NLP\) on SageMaker\. You can use Hugging Face for both training and inference\. This functionality is available through the development of Hugging Face [AWS Deep Learning Containers](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/what-is-dlc.html)\. These containers include Hugging Face Transformers, Tokenizers and the Datasets library, which allows you to use these resources for your training and inference jobs\. For a list of the available Deep Learning Containers images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. These Deep Learning Containers images are maintained and regularly updated with security patches\.

To use the Hugging Face Deep Learning Containers with the SageMaker Python SDK for training, see the [Hugging Face SageMaker Estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/index.html)\. With the Hugging Face Estimator, you can use the Hugging Face models as you would any other SageMaker Estimator\. However, using the SageMaker Python SDK is optional\. You can also orchestrate your use of the Hugging Face Deep Learning Containers with the AWS CLI and AWS SDK for Python \(Boto3\)\.

For more information on Hugging Face and the models available in it, see the [Hugging Face documentation](https://huggingface.co/)\. 

## Training<a name="hugging-face-training"></a>

To run training, you can use any of the thousands of models available in Hugging Face and fine\-tune them for your specific use case with additional training\. With SageMaker, you can use standard training or take advantage of [SageMaker Distributed Data and Model Parallel training](https://docs.aws.amazon.com/sagemaker/latest/dg/distributed-training.html)\. As with other SageMaker training jobs using custom code, you can capture your own metrics by passing a metrics definition to the SageMaker Python SDK as shown in [Defining Training Metrics \(SageMaker Python SDK\) ](https://docs.aws.amazon.com/sagemaker/latest/dg/training-metrics.html#define-train-metrics-sdk) \. The captured metrics are then accessible via [CloudWatch](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html) and as a Pandas `DataFrame` via the [TrainingJobAnalytics](https://sagemaker.readthedocs.io/en/stable/api/training/analytics.html#sagemaker.analytics.TrainingJobAnalytics) method\. Once your model is trained and fine\-tuned, you can use it like any other model to run inference jobs\.

### How to run training with the Hugging Face Estimator<a name="hugging-face-training-using"></a>

You can implement the Hugging Face Estimator for training jobs using the SageMaker Python SDK\. The SageMaker Python SDK is an open source library for training and deploying machine learning models on SageMaker\. For more information on the Hugging Face Estimator, see the [SageMaker Python SDK documentation\.](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/index.html)

With the SageMaker Python SDK, you can run training jobs using the Hugging Face Estimator in the following environments: 
+ [SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html): Amazon SageMaker Studio is the first fully integrated development environment \(IDE\) for machine learning \(ML\)\. SageMaker Studio provides a single, web\-based visual interface where you can perform all ML development steps required to prepare, build, train and tune, deploy and manage models\. For information on using Jupyter Notebooks in Studio, see [Use Amazon SageMaker Studio Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks.html)\.
+ [SageMaker Notebook Instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html): An Amazon SageMaker notebook instance is a machine learning \(ML\) compute instance running the Jupyter Notebook App\. This app lets you run Jupyter Notebooks in your notebook instance to prepare and process data, write code to train models, deploy models to SageMaker hosting, and test or validate your models without SageMaker Studio features like Debugger, Model Monitoring, and a web\-based IDE\.
+ Locally: If you have connectivity to AWS and have appropriate SageMaker permissions, you can use the SageMaker Python SDK locally to launch remote training and inference jobs for Hugging Face in SageMaker on AWS\. This works on your local machine, as well as other AWS services with a connected SageMaker Python SDK and appropriate permissions\.

## Inference<a name="hugging-face-inference"></a>

For inference, you can use your trained Hugging Face model or one of the pretrained Hugging Face models to deploy an inference job with SageMaker\. With this collaboration, you only need one line of code to deploy both your trained models and pre\-trained models with SageMaker\. You can also run inference jobs without having to write any custom inference code\. With custom inference code, you can customize the inference logic by providing your own Python script\.

### How to deploy an inference job using the Hugging Face Deep Learning Containers<a name="hugging-face-inference-using"></a>

You have two options for running inference with SageMaker\. You can run inference using a model that you trained, or deploy a pre\-trained Hugging Face model\. 
+ **Run inference with your trained model:** You have two options for running inference with your own trained model\. You can run inference with a model that you trained using an existing Hugging Face model with the SageMaker Hugging Face Deep Learning Containers, or you can bring your own existing Hugging Face model and deploy it using SageMaker\. When you run inference with a model that you trained with the SageMaker Hugging Face Estimator, you can deploy the model immediately after training completes or you can upload the trained model to an Amazon S3 bucket and ingest it when running inference later\. If you bring your own existing Hugging Face model, you must upload the trained model to an Amazon S3 bucket and ingest that bucket when running inference as shown in [Deploy your Hugging Face Transformers for inference example](https://github.com/huggingface/notebooks/blob/master/sagemaker/10_deploy_model_from_s3/deploy_transformer_model_from_s3.ipynb)\.
+ **Run inference with a pre\-trained HuggingFace model: **You can use one of the thousands of pre\-trained Hugging Face models to run your inference jobs with no additional training needed\. To run inference, you select the pre\-trained model from the list of [Hugging Face models](https://huggingface.co/models), as outlined in [Deploy pre\-trained Hugging Face Transformers for inference example](https://github.com/huggingface/notebooks/blob/master/sagemaker/11_deploy_model_from_hf_hub/deploy_transformer_model_from_hf_hub.ipynb)\.

## What do you want to do?<a name="hugging-face-do"></a>

The following Jupyter Notebooks in the Hugging Face notebooks repository illustrate how to use the Hugging Face Deep Learning Containers with SageMaker in various use cases\.

I want to train and deploy a text classification model using Hugging Face in SageMaker with PyTorch\.  
For a sample Jupyter Notebook, see the [PyTorch Getting Started Demo](https://github.com/huggingface/notebooks/blob/master/sagemaker/01_getting_started_pytorch/sagemaker-notebook.ipynb)\.

I want to train and deploy a text classification model using Hugging Face in SageMaker with TensorFlow\.  
For a sample Jupyter Notebook, see the [TensorFlow Getting Started example](https://github.com/huggingface/notebooks/blob/master/sagemaker/02_getting_started_tensorflow/sagemaker-notebook.ipynb)\.

I want to run distributed training with data parallelism using Hugging Face and SageMaker Distributed\.  
For a sample Jupyter Notebook, see the [Distributed Training example](https://github.com/huggingface/notebooks/blob/master/sagemaker/03_distributed_training_data_parallelism/sagemaker-notebook.ipynb)\.

I want to run distributed training with model parallelism using Hugging Face and SageMaker Distributed\.  
For a sample Jupyter Notebook, see the [Model Parallelism example](https://github.com/huggingface/notebooks/blob/master/sagemaker/04_distributed_training_model_parallelism/sagemaker-notebook.ipynb)\.

I want to use a spot instance to train and deploy a model using Hugging Face in SageMaker\.  
For a sample Jupyter Notebook, see the [Spot Instances example](https://github.com/huggingface/notebooks/blob/master/sagemaker/05_spot_instances/sagemaker-notebook.ipynb)\.

I want to capture custom metrics and use SageMaker Checkpointing when training a text classification model using Hugging Face in SageMaker\.  
For a sample Jupyter Notebook, see the [Training with Custom Metrics example](https://github.com/huggingface/notebooks/blob/master/sagemaker/06_sagemaker_metrics/sagemaker-notebook.ipynb)\.

I want to train a distributed question\-answering TensorFlow model using Hugging Face in SageMaker\.  
For a sample Jupyter Notebook, see the [Distributed TensorFlow Training example](https://github.com/huggingface/notebooks/blob/master/sagemaker/07_tensorflow_distributed_training_data_parallelism/sagemaker-notebook.ipynb)\.

I want to train a distributed summarization model using Hugging Face in SageMaker\.  
For a sample Jupyter Notebook, see the [Distributed Summarization Training example](https://github.com/huggingface/notebooks/blob/master/sagemaker/08_distributed_summarization_bart_t5/sagemaker-notebook.ipynb)\.

I want to train an image classification model using Hugging Face in SageMaker\.  
For a sample Jupyter Notebook, see the [Vision Transformer Training example](https://github.com/huggingface/notebooks/blob/master/sagemaker/09_image_classification_vision_transformer/sagemaker-notebook.ipynb)\.

I want to deploy my trained Hugging Face model in SageMaker\.  
For a sample Jupyter Notebook, see the [Deploy your Hugging Face Transformers for inference example](https://github.com/huggingface/notebooks/blob/master/sagemaker/10_deploy_model_from_s3/deploy_transformer_model_from_s3.ipynb)\.

I want to deploy a pre\-trained Hugging Face model in SageMaker\.  
For a sample Jupyter Notebook, see the [Deploy pre\-trained Hugging Face Transformers for inference example](https://github.com/huggingface/notebooks/blob/master/sagemaker/11_deploy_model_from_hf_hub/deploy_transformer_model_from_hf_hub.ipynb)\.