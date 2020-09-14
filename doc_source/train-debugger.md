# Amazon SageMaker Debugger<a name="train-debugger"></a>

 Amazon SageMaker Debugger provides full visibility into model training by monitoring, recording, analyzing, and visualizing training process tensors\. A *tensor* is a high\-dimensional array of machine learning and deep learning metrics such as weights, gradients, and losses\. It is a collection of metrics that's continuously updated during the backpropagation and optimization process of training deep learning models\. 

## Debugger Overview<a name="debugger-overview"></a>

 Debugger can dramatically reduce the time, resources, and cost needed to train models\. By using Debugger in Amazon SageMaker Studio or Amazon SageMaker notebook instances, you can use the supported features and frameworks to inspect training job issues and use a visual interface analyze your tensor data\. 

 SageMaker Debugger Python SDK and its client library `smdebug` are designed to create Python objects that enable you to interact with the saved tensors\. Debugger provides tools to set up *hooks* and *rules* to save and access tensors, and to make the tensors available for analysis through its *trial* feature, all through flexible and powerful API operations\. It supports the machine learning frameworks TensorFlow, PyTorch, MXNet, and XGBoost on Python 3\.6 and later\. 

If you want to find direct resources of the Debugger and `smdebug` API operations, see the following documentations and pages: 
+ [Amazon SageMaker Debugger Python SDK](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_debugger.html)
+ [The Debugger API](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html)
+ [The open source client library smdebug](https://github.com/awslabs/sagemaker-debugger#amazon-sagemaker-debugger)
+ [The smdebug PyPI \- the latest version release](https://pypi.org/project/smdebug/)

## Debugger\-supported Frameworks with AWS Deep Learning Containers and Built\-in SageMaker Algorithms<a name="debugger-supported-aws-containers"></a>

To enable Amazon SageMaker Debugger, use one of the pre\-built [AWS Deep Learning Containers Images](https://github.com/aws/deep-learning-containers#aws-deep-learning-containers) and [the SageMaker XGBoost built\-in algorithm](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) listed in the following tables\. 

### Available Frameworks to Use Debugger<a name="debugger-zero-script-change-table"></a>

Available frameworks of AWS Deep Learning Containers and XGBoost to use Debugger\. Each of these pre\-built containers already has the Debugger feature installed, and you can directly run your training script without any change and debug training jobs\. If you use one of the frameworks fully supported by Debugger, your training job will be automatically configured with pre\-installed hook registrations to your training script\. To find the lasted version updates and release notes, see the following table and links to the frameworks or algorithms that you want to use\.


| Framework | Versions | 
| --- | --- | 
|  [TensorFlow](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/tensorflow.md)  |  1\.15, 2\.1\*, 2\.2, 2\.3\*  | 
|  [MXNet](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/mxnet.md)  |  1\.6  | 
|  [PyTorch](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/pytorch.md)  |  1\.4, 1\.5, 1\.6  | 
|  [XGBoost](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md) \([as a built\-in algorithm](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md#use-xgboost-as-a-built-in-algorithm)\)   |  0\.90\-2, 1\.0\-1  | 

To start using the AWS containers and Debugger, see [Use Debugger in AWS Containers](debugger-container.md)\.

\* Debugger with zero script change is partially available for these TensorFlow versions\. The `inputs`, `outputs`, `gradients`, and `layers` built\-in collections are currently not available for these TensorFlow versions\.

### Available Frameworks to Use Debugger with Script Mode<a name="debugger-script-mode-table"></a>

If you want to use Debugger with a framework of a version not listed in the previous table or manually control the Debugger hook registration in your training script, you need to run your training job with script mode on the SageMaker training containers\. To find more information about available SageMaker containers and how to register the Debugger hooks using the `smdebug` library, see the following table and links to the frameworks or algorithms that you want to use\.


| Framework | Versions | 
| --- | --- | 
|  [TensorFlow](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/tensorflow.md#using-debugger-on-sagemaker-training-containers-and-custom-containers)  |  1\.13, 1\.14, 1\.15, 2\.1, 2\.2, 2\.3  | 
|  [Keras with TensorFlow backend](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/tensorflow.md#using-debugger-on-sagemaker-training-containers-and-custom-containers)  |  2\.3  | 
|  [MXNet](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/mxnet.md)  |  1\.4, 1\.5, 1\.6  | 
|  [PyTorch](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/pytorch.md)  |  1\.2, 1\.3, 1\.4, 1\.5, 1\.6  | 
|  [XGBoost](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md) \(as a framework\)  |  0\.90\-2, 1\.0\-1  | 

To start using the AWS containers and Debugger with script mode, go to [Debugger in AWS Containers with Script Mode](debugger-container.md#debugger-script-mode)\.

### Use Debugger with Custom Containers<a name="debugger-byoc-intro"></a>

Amazon SageMaker Debugger is available for any deep learning models that you bring to SageMaker\. The AWS CLI, SageMaker `Estimator` API, and the Debugger APIs enable you to use any Docker base images to build and customize containers to train and debug your models\.

To use Debugger with customized containers, see [Use Debugger with Custom Training Containers](debugger-bring-your-own-container.md)\.

## How Debugger Works<a name="debugger-how-it-works"></a>

With Amazon SageMaker Debugger, you can go beyond just looking at scalars, like losses and accuracies, when evaluating model training\. Debugger gives you full visibility into a training job by using a *hook* to capture tensors that define the state of the training process at each point in the job's lifecycle\. Debugger also provides *rules* to inspect the captured tensors\. 

Built\-in *rules* monitor the training flow and alert you to problems with common conditions that are critical for the success of the training job\. You can also create your own custom rules to watch for any issues specific to your model\. You can monitor the results of the analysis done by rules with Amazon CloudWatch events, using an Amazon SageMaker notebook, or using visualizations provided by Amazon SageMaker Studio\.

The following diagram shows the flow for the model training process with Amazon SageMaker Debugger\.

![\[Overview of how Amazon SageMaker Debugger works.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/how-debugger-works-4.png)

To use Amazon SageMaker Debugger hooks and rules, you activate them by adding only a few lines of code in your estimator object\. Debugger and its client library `smdebug` help you set up hooks and rules that give you transparency into training jobs\. Debugger and `smdebug` support the major machine learning frameworks \(that is, TensorFlow, MXNet, and PyTorch\) and the SageMaker pre\-built algorithm XGBoost, while you run training jobs in the SageMaker environment\. 
+ **Choose a framework and use SageMaker pre\-built training containers – ** SageMaker provides you with the option to use AWS Deep Learning Containers, the XGBoost built\-in algorithm container, or custom containers to run training jobs\. Combination of Debugger features and the pre\-built AWS training containers helps make your model debugging process simpler and more transparent\. You can build containers using Deep Learning Containers base images if you prefer to customize your training job and enable Debugger using the SageMaker Python SDK\. To learn more, go to [Use Debugger in AWS Containers](debugger-container.md) and [Use Debugger with Custom Training Containers](debugger-bring-your-own-container.md)\. 
+ **Use a Debugger hook to save tensors – ** After you choose a container and a framework that fit your training script, use a Debugger hook to configure which tensors to save and to which directory to save them, such as a Amazon S3 bucket\. A Debugger hook helps you to build the configuration and to keep it in your account to use in subsequent analyses, where it is secured for use with the most privacy\-sensitive applications\. To learn more, see [Configure and Save Tensor Data Using the Debugger API Operations](debugger-data.md)\. 
+ **Use Debugger rules to inspect tensors in parallel with a training job – ** To analyze tensors, Debugger provides built\-in rules for over a dozen abnormal training process behaviors\. For example, a Debugger rule detects issues when the training process suffers from vanishing gradients, exploding tensors, overfitting, or overtraining\. If necessary, you can build customized rules to analyze saved tensors using the Amazon SageMaker Debugger SDK\. To learn more about the Debugger rules, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md) for detailed instructions and code examples, [List of Debugger Built\-in Rules](debugger-built-in-rules.md) for a full list of the Debugger rules, and [Create Debugger Custom Rules for Training Job Analysis](debugger-custom-rules.md) for customizing Debugger rules\. In combination with Debugger rules, you can also use Amazon CloudWatch Events to invoke an AWS Lambda function that automatically stops the training job when the Debugger rules detect problems and trigger `"IssuesFound"` status\. To configure the automated training job termination using Debugger, see [Action on Amazon SageMaker Debugger Rules Using Amazon CloudWatch and AWS Lambda](debugger-cloudwatch-lambda.md)\. 
+ **Create *trial*s to analyze tensors – **The `smdebug` trial is an object that lets you query the saved tensors from a given training job, specified by the path to which `smdebug` artifacts are saved\. A trial is capable of loading new tensors as they become available at a given path, enabling you to do both offline and real\-time analysis\. To learn more about the smdebug trial, see [ the smdebug trial API](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/analysis.md#trial-api)\. 
+ **Use SageMaker Studio for visualization –** You can use Debugger in SageMaker Studio to visualize collected trials by Debugger\. SageMaker Studio makes inspecting training job issues easier through its visual interface for analyzing your tensor data\. To learn more, see [Amazon SageMaker Studio Visualization Demos of Model Analysis with Debugger](debugger-visualization.md)\. 

## Amazon SageMaker Debugger Tutorial Videos<a name="debugger-videos"></a>

The following video series provides a tour of Amazon SageMaker Debugger capabilities using SageMaker Studio and SageMaker notebook instances\. 

**Topics**
+ [Debug Models with Amazon SageMaker Debugger in Studio](#debugger-video-get-started)
+ [Deep Dive on Amazon SageMaker Debugger and SageMaker Model Monitor](#debugger-video-dive-deep)

### Debug Models with Amazon SageMaker Debugger in Studio<a name="debugger-video-get-started"></a>

*Julien Simon, AWS Technical Evangelist \| Length: 14 minutes 17 seconds*

This tutorial video demonstrates how to use Amazon SageMaker Debugger to capture and inspect debugging information from a training model\. The example training model used in this video is a simple convolutional neural network \(CNN\) based on Keras with the TensorFlow backend\. SageMaker in a TensorFlow framework and Debugger enable you to build an estimator directly using the training script and debug the training job\.

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/MqPdTj0Znwg/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/MqPdTj0Znwg)

You can find the example notebook in the video in [ this Studio Demo repository](https://gitlab.com/juliensimon/amazon-studio-demos/-/tree/master) provided by the author\. You need to clone the `debugger.ipynb` notebook file and the `mnist_keras_tf.py` training script to your SageMaker Studio or a SageMaker notebook instance\. After you clone the two files, specify the path `keras_script_path` to the `mnist_keras_tf.py` file inside the `debugger.ipynb` notebook\. For example, if you cloned the two files in the same directory, set it as `keras_script_path = "mnist_keras_tf.py"`\.

### Deep Dive on Amazon SageMaker Debugger and SageMaker Model Monitor<a name="debugger-video-dive-deep"></a>

*Julien Simon, AWS Technical Evangelist \| Length: 44 minutes 34 seconds*

This video session explores advanced features of Debugger and SageMakerModel Monitor that help boost productivity and the quality of your models\. First, this video shows how to detect and fix training issues, visualize tensors, and improve models with Debugger\. Next, at 22:41, the video shows how to monitor models in production and identify prediction issues such as missing features or data drift using SageMaker Model Monitor\. Finally, it offers cost optimization tips to help you make the most of your machine learning budget\.

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/0zqoeZxakOI/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/0zqoeZxakOI)

You can find the example notebook in the video in [ this AWS Dev Days 2020 repository](https://gitlab.com/juliensimon/awsdevdays2020/-/tree/master/mls1) offered by the author\.

**Topics**
+ [Debugger Overview](#debugger-overview)
+ [Debugger\-supported Frameworks with AWS Deep Learning Containers and Built\-in SageMaker Algorithms](#debugger-supported-aws-containers)
+ [How Debugger Works](#debugger-how-it-works)
+ [Amazon SageMaker Debugger Tutorial Videos](#debugger-videos)
+ [Amazon SageMaker Studio Visualization Demos of Model Analysis with Debugger](debugger-visualization.md)
+ [Use Debugger in AWS Containers](debugger-container.md)
+ [Configure and Save Tensor Data Using the Debugger API Operations](debugger-data.md)
+ [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)
+ [List of Debugger Built\-in Rules](debugger-built-in-rules.md)
+ [Create Debugger Custom Rules for Training Job Analysis](debugger-custom-rules.md)
+ [Action on Amazon SageMaker Debugger Rules Using Amazon CloudWatch and AWS Lambda](debugger-cloudwatch-lambda.md)
+ [Amazon SageMaker Debugger Advanced Topics and Reference Documentation](debugger-reference.md)