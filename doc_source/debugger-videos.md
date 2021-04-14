# Debugger Tutorial Videos<a name="debugger-videos"></a>

The following videos provide a tour of Amazon SageMaker Debugger capabilities using SageMaker Studio and SageMaker notebook instances\. 

**Topics**
+ [Analyze, Detect, and Get Alerted on Problems with Training Runs Using Amazon SageMaker Debugger](#debugger-video-get-started)
+ [Debug Models with Amazon SageMaker Debugger in Studio](#debugger-video-get-started)
+ [Deep Dive on Amazon SageMaker Debugger and SageMaker Model Monitor](#debugger-video-dive-deep)

## Analyze, Detect, and Get Alerted on Problems with Training Runs Using Amazon SageMaker Debugger<a name="debugger-video-get-started"></a>

*Emily Webber, AWS Machine Learning Specialist \| Length: 13 minutes 54 seconds*

This tutorial video gives you a tour of Amazon SageMaker Debugger to capture, debug, and visualize model output data from a training model with MXNet\. Learn how Amazon SageMaker Debugger makes the training process transparent by automatically capturing metrics, analyzing training runs, and detecting problems\. 

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/0pDEAbof_To/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/0pDEAbof_To)

You can find the example notebook in this video at [Visualizing Debugging Tensors of MXNet training](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/mnist_tensor_plot/mnist-tensor-plot.html) in the [Amazon SageMaker Examples](https://sagemaker-examples.readthedocs.io/en/latest/) GitHub repository\.

## Debug Models with Amazon SageMaker Debugger in Studio<a name="debugger-video-get-started"></a>

*Julien Simon, AWS Technical Evangelist \| Length: 14 minutes 17 seconds*

This tutorial video demonstrates how to use Amazon SageMaker Debugger to capture and inspect debugging information from a training model\. The example training model used in this video is a simple convolutional neural network \(CNN\) based on Keras with the TensorFlow backend\. SageMaker in a TensorFlow framework and Debugger enable you to build an estimator directly using the training script and debug the training job\.

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/MqPdTj0Znwg/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/MqPdTj0Znwg)

You can find the example notebook in the video in [ this Studio Demo repository](https://gitlab.com/juliensimon/amazon-studio-demos/-/tree/master) provided by the author\. You need to clone the `debugger.ipynb` notebook file and the `mnist_keras_tf.py` training script to your SageMaker Studio or a SageMaker notebook instance\. After you clone the two files, specify the path `keras_script_path` to the `mnist_keras_tf.py` file inside the `debugger.ipynb` notebook\. For example, if you cloned the two files in the same directory, set it as `keras_script_path = "mnist_keras_tf.py"`\.

## Deep Dive on Amazon SageMaker Debugger and SageMaker Model Monitor<a name="debugger-video-dive-deep"></a>

*Julien Simon, AWS Technical Evangelist \| Length: 44 minutes 34 seconds*

This video session explores advanced features of Debugger and SageMaker Model Monitor that help boost productivity and the quality of your models\. First, this video shows how to detect and fix training issues, visualize tensors, and improve models with Debugger\. Next, at 22:41, the video shows how to monitor models in production and identify prediction issues such as missing features or data drift using SageMaker Model Monitor\. Finally, it offers cost optimization tips to help you make the most of your machine learning budget\.

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/0zqoeZxakOI/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/0zqoeZxakOI)

You can find the example notebook in the video in [ this AWS Dev Days 2020 repository](https://gitlab.com/juliensimon/awsdevdays2020/-/tree/master/mls1) offered by the author\.