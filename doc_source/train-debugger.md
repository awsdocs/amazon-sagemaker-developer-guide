# Amazon SageMaker Debugger<a name="train-debugger"></a>

 Amazon SageMaker Debugger provides full visibility into model training by monitoring, recording, analyzing, and visualizing training process tensors\. A *tensor* is a high\-dimensional array of machine learning and deep learning metrics such as weights, gradients, and losses; in other words, it is a collection of metrics continuously updated during the backpropagation and optimization process of training deep learning models\. 

## Debugger Overview<a name="debugger-overview"></a>

 Debugger can dramatically reduce the time, resources, and cost needed to train models\. Using Debugger in Amazon SageMaker Studio or Amazon SageMaker Notebook instances makes inspecting training job issues easier with supported features and frameworks and provides a visual interface for analyzing your tensor data\. 

 Amazon SageMaker Debugger Python SDK and its client library `smdebug` are designed to create Python objects that enable you to interact with the saved tensors\. Debugger provides tools to set up *hooks* and *rules* to save and access tensors, as well as to make the tensors available for analysis through its *trial* feature, all through flexible and powerful APIs\. It supports the machine learning frameworks **TensorFlow**, **PyTorch**, **MXNet**, and **XGBoost** on Python 3\.6 and above\. 

If you want to learn more about the Debugger and `smdebug` APIs, see the following documentation: 
+ [Amazon SageMaker Debugger Python SDK](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_debugger.html)
+ [The Debugger API](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html)
+ [The open source client library smdebug](https://github.com/awslabs/sagemaker-debugger#amazon-sagemaker-debugger)

## Amazon SageMaker Debugger Sample Notebooks<a name="debugger-sample-notebooks"></a>

The following Amazon SageMaker Debugger sample notebooks show how to set up Amazon SageMaker training jobs to configure Debugger hooks to save the tensors, apply Debugger rules to the tensors to monitor the status of training jobs, and visualize them\. 

The notebooks are best used in the following order:
+ [Using a Built\-in Rule with TensorFlow](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_builtin_rule)
+ [Using a Custom Rule with TensorFlow Keras](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_keras_custom_rule)
+ [Real\-time Analysis in Notebook with MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_realtime_analysis)
+ [Using a Built\-in Rule with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_builtin_rules)
+ [Real\-time Analysis in Notebook with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_realtime_analysis)
+ [Using SageMaker Debugger with Managed Spot Training and MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_spot_training)
+ [Trigger Amazon CloudWatch Events using Debugger Rules to Take an Action Based on Training Status with TensorFlow](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_action_on_rule)

## Amazon SageMaker Debugger Tutorial Videos<a name="debugger-videos"></a>

The following video series provides a tour of Amazon SageMaker Debugger capabilities using Amazon SageMaker Studio and Amazon SageMaker Notebook instances\. 

**Topics**
+ [Debug Models with Amazon SageMaker Debugger in Studio](#debugger-video-get-started)
+ [Deep Dive on Amazon SageMaker Debugger and Amazon SageMaker Model Monitor](#debugger-video-dive-deep)

### Debug Models with Amazon SageMaker Debugger in Studio<a name="debugger-video-get-started"></a>

*Julien Simon, AWS Technical Evangelist \| Length: 14 minutes 17 seconds*

This tutorial video demonstrates how to use Amazon SageMaker Debugger to capture and inspect debugging information from a training model\. The example training model used in this video is a simple Convolutional Neural Network \(CNN\) based on Keras with the TensorFlow backend\. Amazon SageMaker in a TensorFlow framework and Debugger enable you to build an estimator directly using the training script and debug the training job\.

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/MqPdTj0Znwg/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/MqPdTj0Znwg)

You can find the example notebook in the video in [ this Studio Demo repository](https://gitlab.com/juliensimon/amazon-studio-demos/-/tree/master) provided by the author\. You need to clone the `debugger.ipynb` notebook file and a training script `mnist_keras_tf.py` example file to your Amazon SageMaker Studio or a Amazon SageMaker Notebook instance\. After you clone the two files, specify the path `keras_script_path` to the `mnist_keras_tf.py` file inside the `debugger.ipynb` notebook\. If you cloned the two files in the same directory, set it as `keras_script_path = "mnist_keras_tf.py"`\.

### Deep Dive on Amazon SageMaker Debugger and Amazon SageMaker Model Monitor<a name="debugger-video-dive-deep"></a>

*Julien Simon, AWS Technical Evangelist \| Length: 44 minutes 34 seconds*

This video session explores advanced features of Debugger and Amazon SageMakerModel Monitor that help boost productivity and the quality of your models\. First, this video shows how to detect and fix training issues, visualize tensors, and improve models with Debugger\. Next, at 22:41, the video shows how to monitor models in production and identify prediction issues such as missing features or data drift using Amazon SageMaker Model Monitor\. Finally, it offers cost optimization tips to help you make the most of your machine learning budget\.

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/0zqoeZxakOI/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/0zqoeZxakOI)

You can find the example notebook in the video in [ this AWS Dev Days 2020 repository](https://gitlab.com/juliensimon/awsdevdays2020/-/tree/master/mls1) offered by the author\.

**Topics**
+ [Debugger Overview](#debugger-overview)
+ [Amazon SageMaker Debugger Sample Notebooks](#debugger-sample-notebooks)
+ [Amazon SageMaker Debugger Tutorial Videos](#debugger-videos)
+ [How Debugger Works](debugger-how-it-works.md)
+ [Amazon SageMaker Studio Visualizations of Model Analysis Results with Debugger](debugger-visualization.md)
+ [Use Containers in Frameworks Supported by Debugger](debugger-container.md)
+ [Save Tensor Data Using the Debugger Hook API](debugger-data.md)
+ [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)
+ [List of Built\-in Rules Provided by Amazon SageMaker Debugger](debugger-built-in-rules.md)
+ [How to Use Custom Rules](debugger-custom-rules.md)
+ [Use Amazon SageMaker Debugger Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)
+ [Amazon SageMaker Debugger Reference Documentation](debugger-reference.md)