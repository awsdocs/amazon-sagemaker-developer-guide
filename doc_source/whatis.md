# What Is Amazon SageMaker?<a name="whatis"></a>

Amazon SageMaker is a fully managed machine learning service\. With Amazon SageMaker, data scientists and developers can quickly and easily build and train machine learning models, and then directly deploy them into a production\-ready hosted environment\. It provides an integrated Jupyter authoring notebook instance for easy access to your data sources for exploration and analysis, so you don't have to manage servers\. It also provides common machine learning algorithms that are optimized to run efficiently against extremely large data in a distributed environment\. With native support for bring\-your\-own\-algorithms and frameworks, Amazon SageMaker offers flexible distributed training options that adjust to your specific workflows\. Deploy a model into a secure and scalable environment by launching it with a few clicks from Amazon SageMaker Studio or the Amazon SageMaker console\. Training and hosting are billed by minutes of usage, with no minimum fees and no upfront commitments\.

This guide includes information and tutorials on Amazon SageMaker features\. To learn how to build, train, and deploy models using Amazon SageMaker, see [Amazon SageMaker developer resources](https://aws.amazon.com/sagemaker/developer-resources/)\. 

**Topics**
+ [Amazon SageMaker Features](#whatis-features)
+ [Amazon SageMaker Pricing](#whatis-pricing)
+ [Are You a First\-time User of Amazon SageMaker?](#first-time-user)

## Amazon SageMaker Features<a name="whatis-features"></a>

Amazon SageMaker includes the following features:

**[Amazon SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio.html)**  
An integrated machine learning environment where you can build, train, deploy, and analyze your models all in the same application\.

**[Amazon SageMaker Ground Truth](https://docs.aws.amazon.com/sagemaker/latest/dg/sms.html)**  
High\-quality training datasets by using workers along with machine learning to create labeled datasets\.

**[Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/use-augmented-ai-a2i-human-review-loops.html)**  
Human\-in\-the\-loop reviews

**[Amazon SageMaker Studio Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks.html)**  
The next generation of Amazon SageMaker notebooks that include SSO integration, fast start\-up times, and single\-click sharing\.

**[Preprocessing](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)**  
Analyze and pre\-process data, tackle feature engineering, and evaluate models\.

**[Amazon SageMaker Experiments](https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html)**  
Experiment management and tracking\. You can use the tracked data to reconstruct an experiment, incrementally build on experiments conducted by peers, and trace model lineage for compliance and audit verifications\.

**[Amazon SageMaker Debugger](https://docs.aws.amazon.com/sagemaker/latest/dg/train-debugger.html)**  
Inspect training parameters and data throughout the training process\. Automatically detect and alert users to commonly occurring errors such as parameter values getting too large or small\.

**[Amazon SageMaker Autopilot](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-automate-model-development.html)**  
Users without machine learning knowledge can quickly build classification and regression models\.

**[Reinforcement Learning](https://docs.aws.amazon.com/sagemaker/latest/dg/reinforcement-learning.html)**  
Maximize the long\-term reward that an agent receives as a result of its actions\.

**[Batch Transform](https://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform.html)**  
Preprocess datasets, run inference when you don't need a persistent endpoint, and associate input records with inferences to assist the interpretation of results\.

**[Amazon SageMaker Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html)**  
Monitor and analyze models in production \(endpoints\) to detect data drift and deviations in model quality\.

**[Amazon SageMaker Neo](https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html)**  
Train machine learning models once, then run anywhere in the cloud and at the edge\.

**[Amazon SageMaker Elastic Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/ei.html)**  
Speed up the throughput and decrease the latency of getting real\-time inferences\.

## Amazon SageMaker Pricing<a name="whatis-pricing"></a>

As with other AWS products, there are no contracts or minimum commitments for using Amazon SageMaker\. For more information about the cost of using Amazon SageMaker, see [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/)\.

## Are You a First\-time User of Amazon SageMaker?<a name="first-time-user"></a>

If you are a first\-time user of Amazon SageMaker, we recommend that you do the following: 

1. **Read [How Amazon SageMaker Works](how-it-works.md)** – This section provides an overview of Amazon SageMaker, explains key concepts, and describes the core components involved in building AI solutions with Amazon SageMaker\. We recommend that you read this topic in the order presented\.

1. **[Set Up Amazon SageMaker](gs-set-up.md)** – This section explains how to set up your AWS account and onboard to Amazon SageMaker Studio\.

1. **[Get Started with Amazon SageMaker](gs.md)** – This section walks you through training your first model using Amazon SageMaker Studio, or the Amazon SageMaker console and the Amazon SageMaker API\. You use training algorithms provided by Amazon SageMaker\.

1. **Explore other topics** – Depending on your needs, do the following:
   + **Submit Python code to train with deep learning frameworks** – In Amazon SageMaker, you can use your own training scripts to train models\. For information, see [Use Machine Learning Frameworks with Amazon SageMaker](frameworks.md)\.
   + **Use Amazon SageMaker directly from Apache Spark** – For information, see [Use Apache Spark with Amazon SageMaker](apache-spark.md)\.
   + **Use Amazon AI to train and/or deploy your own custom algorithms** – Package your custom algorithms with Docker so you can train and/or deploy them in Amazon SageMaker\. See [Use Your Own Algorithms or Models with Amazon SageMaker ](your-algorithms.md) to learn how Amazon SageMaker interacts with Docker containers, and for the Amazon SageMaker requirements for Docker images\. 

1. **View the [API Reference](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Reference.html)** – This section describes the Amazon SageMaker API operations\.