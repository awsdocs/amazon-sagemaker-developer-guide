# Amazon SageMaker Debugger<a name="train-debugger"></a>

Debug, monitor, and profile training jobs in real time, detect non\-converging conditions, optimize resource utilization by eliminating bottlenecks, improve training time, and reduce costs of your machine learning models using Amazon SageMaker Debugger\.

## Amazon SageMaker Debugger Features<a name="debugger-features"></a>

A machine learning \(ML\) training job can have problems such as system bottlenecks, overfitting, saturated activation functions, and vanishing gradients, which can compromise model performance\.

SageMaker Debugger profiles and debugs training jobs to help resolve such problems and improve your ML model's compute resource utilization and performance\. Debugger offers tools to send alerts when training anomalies are found, take actions against the problems, and identify the root cause of them by visualizing collected metrics and tensors\.

SageMaker Debugger supports Apache MXNet, TensorFlow, PyTorch, and XGBoost\. For more information about available frameworks and versions, see [Supported Frameworks and Algorithms](debugger-supported-frameworks.md)\.

![\[Overview of how Amazon SageMaker Debugger works.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-main.png)

The high\-level Debugger workflow is as follows:

1. Configure a SageMaker training job with Debugger\.
   + Configure using the SageMaker [`Estimator` API \(for Python SDK\)](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-configuration.html)\.
   + Configure using the SageMaker [`CreateTrainingJob` request \(for Boto3 or CLI\)](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-createtrainingjob-api.html)\.
   + Configure [custom training containers](debugger-bring-your-own-container.md) with Debugger\.

1. Start a training job and monitor training issues in real time\.
   + [SageMaker Studio Debugger dashboards in Studio Experiments and trials](debugger-on-studio.md)\.
   + [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

1. Get alerts and take prompt actions against the training issues\.
   + Receive texts and emails and stop training jobs when training issues are found using [Debugger Built\-in Actions for Rules](debugger-built-in-actions.md)\.
   + Set up your own actions using [Amazon CloudWatch Events and AWS Lambda](debugger-cloudwatch-lambda.md)\.

1. Receive training reports, suggestions to fix the issues, and insights into your training jobs\.
   + [Studio Debugger Insights dashboard for deep learning frameworks](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-on-studio-insights.html)
   + [Deep learning framework profiling report](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-profiling-report.html)
   + [SageMaker XGBoost training report](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-training-xgboost-report.html)

1. Explore deep analysis of the training issues and bottlenecks\.
   + For profiling training jobs, see [Analyze Data Using the SMDebug Client Library](debugger-analyze-data.md)\.
   + For debugging model parameters, see [Visualize Debugger Output Tensors in TensorBoard](debugger-enable-tensorboard-summaries.md#debugger-enable-tensorboard-summaries.title)\.

1. Fix the issues, considering the suggestions provided by Debugger, and repeat steps 1–5 until you optimize your model and achieve target accuracy\.

The SageMaker Debugger developer guide walks you through the following topics\.

**Topics**
+ [Amazon SageMaker Debugger Features](#debugger-features)
+ [Supported Frameworks and Algorithms](debugger-supported-frameworks.md)
+ [Amazon SageMaker Debugger Architecture](debugger-how-it-works.md)
+ [Get Started with Debugger Tutorials](debugger-tutorial.md)
+ [Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration.md)
+ [Configure Debugger Using Amazon SageMaker API](debugger-createtrainingjob-api.md)
+ [List of Debugger Built\-in Rules](debugger-built-in-rules.md)
+ [Create Debugger Custom Rules for Training Job Analysis](debugger-custom-rules.md)
+ [Use Debugger with Custom Training Containers](debugger-bring-your-own-container.md)
+ [Action on Amazon SageMaker Debugger Rules](debugger-action-on-rules.md)
+ [SageMaker Debugger on Studio](debugger-on-studio.md)
+ [SageMaker Debugger Interactive Reports](debugger-report.md)
+ [Analyze Data Using the SMDebug Client Library](debugger-analyze-data.md)
+ [Visualize Amazon SageMaker Debugger Output Tensors in TensorBoard](debugger-enable-tensorboard-summaries.md)
+ [Best Practices for Amazon SageMaker Debugger](debugger-best-practices.md)
+ [Amazon SageMaker Debugger Advanced Topics and Reference Documentation](debugger-reference.md)