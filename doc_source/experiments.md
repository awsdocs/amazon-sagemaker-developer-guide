# Manage Machine Learning with Amazon SageMaker Experiments<a name="experiments"></a>

Amazon SageMaker Experiments is a capability of Amazon SageMaker that lets you create, manage, analyze, and compare your machine learning experiments\. 

**Experimentation in machine learning**

Machine learning is an iterative process\. You need to experiment with multiple combinations of data, algorithms, and parameters, all while observing the impact of incremental changes on model accuracy\. Over time, this iterative experimentation can result in thousands of model training runs and model versions\. This makes it hard to track the best performing models and their input configurations\. It’s also difficult to compare active experiments with past experiments to identify opportunities for further incremental improvements\. Use SageMaker Experiments to organize, view, analyze, and compare iterative ML experimentation to gain comparative insights and track your best performing models\.

**Manage ML experimentation with SageMaker Experiments**

SageMaker Experiments automatically tracks the inputs, parameters, configurations, and results of your iterations as *runs*\. You can assign, group, and organize these runs into *experiments*\. SageMaker Experiments is integrated with Amazon SageMaker Studio, providing a visual interface to browse your active and past experiments, compare runs on key performance metrics, and identify the best performing models\. SageMaker Experiments tracks all of the steps and artifacts that went into creating a model, and you can quickly revisit the origins of a model when you are troubleshooting issues in production, or auditing your models for compliance verifications\.

Use SageMaker Experiments to view, manage, analyze, and compare both custom experiments that you programmatically create and experiments automatically created from SageMaker jobs\. 

## Supported AWS Regions<a name="experiments-regions"></a>

SageMaker Experiments is generally available in all AWS commercial [Regions](https://docs.aws.amazon.com/sagemaker/latest/dg/regions-quotas.html) where Amazon SageMaker Studio is available, except the China Regions\.

**Topics**
+ [Supported AWS Regions](#experiments-regions)
+ [Create an Amazon SageMaker Experiment](experiments-create.md)
+ [View, search, and compare experiment runs](experiments-view-compare.md)
+ [SageMaker integrations](experiments-sm-integrations.md)
+ [Example notebooks for Amazon SageMaker Experiments](experiments-tutorials.md)
+ [Monitor experiment training metrics with AWS CloudTrail](experiments-monitoring.md)
+ [Clean Up Amazon SageMaker Experiment Resources](experiments-cleanup.md)
+ [Additional supported SDK](experiments-additional-sdk.md)
+ [Experiments FAQs](experiment-faq.md)
+ [Search Using the Amazon SageMaker Console and API](search.md)