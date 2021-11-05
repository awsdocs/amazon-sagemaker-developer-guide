# What Is Amazon SageMaker?<a name="whatis"></a>

Amazon SageMaker is a fully managed machine learning service\. With SageMaker, data scientists and developers can quickly and easily build and train machine learning models, and then directly deploy them into a production\-ready hosted environment\. It provides an integrated Jupyter authoring notebook instance for easy access to your data sources for exploration and analysis, so you don't have to manage servers\. It also provides common machine learning algorithms that are optimized to run efficiently against extremely large data in a distributed environment\. With native support for bring\-your\-own\-algorithms and frameworks, SageMaker offers flexible distributed training options that adjust to your specific workflows\. Deploy a model into a secure and scalable environment by launching it with a few clicks from SageMaker Studio or the SageMaker console\. Training and hosting are billed by minutes of usage, with no minimum fees and no upfront commitments\.

This guide includes information and tutorials on SageMaker features\. For additional information, see [Amazon SageMaker developer resources](https://aws.amazon.com/sagemaker/developer-resources/)\.

**Topics**
+ [Amazon SageMaker Features](#whatis-features)
+ [Amazon SageMaker Pricing](#whatis-pricing)
+ [Are You a First\-time User of Amazon SageMaker?](#first-time-user)

## Amazon SageMaker Features<a name="whatis-features"></a>

Amazon SageMaker includes the following features:

**[SageMaker Studio](studio.md)**  
An integrated machine learning environment where you can build, train, deploy, and analyze your models all in the same application\.

**[SageMaker Model Registry](model-registry.md)**  
Versioning, artifact and lineage tracking, approval workflow, and cross account support for deployment of your machine learning models\.

**[SageMaker Projects](sagemaker-projects.md)**  
Create end\-to\-end ML solutions with CI/CD by using SageMaker projects\.

**[SageMaker Model Building Pipelines](pipelines.md)**  
Create and manage machine learning pipelines integrated directly with SageMaker jobs\.

**[SageMaker ML Lineage Tracking](lineage-tracking.md)**  
Track the lineage of machine learning workflows\.

**[SageMaker Data Wrangler](data-wrangler.md)**  
Import, analyze, prepare, and featurize data in SageMaker Studio\. You can integrate Data Wrangler into your machine learning workflows to simplify and streamline data pre\-processing and feature engineering using little to no coding\. You can also add your own Python scripts and transformations to customize your data prep workflow\.

**[SageMaker Feature Store](feature-store.md)**  
A centralized store for features and associated metadata so features can be easily discovered and reused\. You can create two types of stores, an Online or Offline store\. The Online Store can be used for low latency, real\-time inference use cases and the Offline Store can be used for training and batch inference\.

**[SageMaker JumpStart](studio-jumpstart.md)**  
Learn about SageMaker features and capabilities through curated 1\-click solutions, example notebooks, and pretrained models that you can deploy\. You can also fine\-tune the models and deploy them\.

**[SageMaker Clarify](clarify-fairness-and-explainability.md)**  
Improve your machine learning models by detecting potential bias and help explain the predictions that models make\.

**[SageMaker Edge Manager](edge.md)**  
Optimize custom models for edge devices, create and manage fleets and run models with an efficient runtime\.

**[SageMaker Ground Truth](sms.md)**  
High\-quality training datasets by using workers along with machine learning to create labeled datasets\.

**[Amazon Augmented AI](a2i-use-augmented-ai-a2i-human-review-loops.md)**  
Build the workflows required for human review of ML predictions\. Amazon A2I brings human review to all developers, removing the undifferentiated heavy lifting associated with building human review systems or managing large numbers of human reviewers\.

**[SageMaker Studio Notebooks](notebooks.md)**  
The next generation of SageMaker notebooks that include AWS Single Sign\-On \(AWS SSO\) integration, fast start\-up times, and single\-click sharing\.

**[SageMaker Experiments](experiments.md)**  
Experiment management and tracking\. You can use the tracked data to reconstruct an experiment, incrementally build on experiments conducted by peers, and trace model lineage for compliance and audit verifications\.

**[SageMaker Debugger](train-debugger.md)**  
Inspect training parameters and data throughout the training process\. Automatically detect and alert users to commonly occurring errors such as parameter values getting too large or small\.

**[SageMaker Autopilot](autopilot-automate-model-development.md)**  
Users without machine learning knowledge can quickly build classification and regression models\.

**[SageMaker Model Monitor](model-monitor.md)**  
Monitor and analyze models in production \(endpoints\) to detect data drift and deviations in model quality\.

**[SageMaker Neo](neo.md)**  
Train machine learning models once, then run anywhere in the cloud and at the edge\.

**[SageMaker Elastic Inference](ei.md)**  
Speed up the throughput and decrease the latency of getting real\-time inferences\.

**[Reinforcement Learning](reinforcement-learning.md)**  
Maximize the long\-term reward that an agent receives as a result of its actions\.

**[Preprocessing](processing-job.md)**  
Analyze and preprocess data, tackle feature engineering, and evaluate models\.

**[Batch Transform](batch-transform.md)**  
Preprocess datasets, run inference when you don't need a persistent endpoint, and associate input records with inferences to assist the interpretation of results\.

## Amazon SageMaker Pricing<a name="whatis-pricing"></a>

As with other AWS products, there are no contracts or minimum commitments for using Amazon SageMaker\. For more information about the cost of using SageMaker, see [SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

## Are You a First\-time User of Amazon SageMaker?<a name="first-time-user"></a>

If you are a first\-time user of SageMaker, we recommend that you do the following: 

1. **Read [How Amazon SageMaker Works](#how-it-works)** – This section provides an overview of SageMaker, explains key concepts, and describes the core components involved in building AI solutions with SageMaker\. We recommend that you read this topic in the order presented\.

1. **[Set Up Amazon SageMaker Prerequisites](gs-set-up.md)** – This section explains how to set up your AWS account\.

1. Amazon SageMaker Autopilot simplifies the machine learning experience by automating machine learning tasks\. If you are new to SageMaker, it provides the easiest learning path\. It also serves as an excellent ML learning tool that provides visibility into the code with notebooks generated for each of the automated ML tasks\. For an introduction to its capabilities, see [Automate model development with Amazon SageMaker Autopilot](autopilot-automate-model-development.md)\. To get started building, training, and deploying machine learning models, Autopilot provides:
   + [Samples: Explore modeling with Amazon SageMaker Autopilot](autopilot-samples.md)
   + [Videos: Use Autopilot to automate and explore the machine learning process](autopilot-videos.md)
   + [Tutorials: Get started with Amazon SageMaker Autopilot](autopilot-tutorials.md)

1. **[Get Started with Amazon SageMaker](gs.md)** – This section walks you through training your first model using SageMaker Studio, or the SageMaker console and the SageMaker API\. You use training algorithms provided by SageMaker\.

1. **Explore other topics** – Depending on your needs, do the following:
   + **Submit Python code to train with deep learning frameworks** – In SageMaker, you can use your own training scripts to train models\. For information, see [Use Machine Learning Frameworks, Python, and R with Amazon SageMaker](frameworks.md)\.
   + **Use SageMaker directly from Apache Spark** – For information, see [Use Apache Spark with Amazon SageMaker](apache-spark.md)\.
   + **Use SageMaker to train and deploy your own custom algorithms** – Package your custom algorithms with Docker so you can train and/or deploy them in SageMaker\. To learn how SageMaker interacts with Docker containers, and for the SageMaker requirements for Docker images, see [Using Docker containers with SageMaker ](docker-containers.md)\. 

1. **View the [API Reference](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Reference.html)** – This section describes the SageMaker API operations\.

### How Amazon SageMaker Works<a name="how-it-works"></a>

SageMaker is a fully managed service that enables you to quickly and easily integrate machine learning\-based models into your applications\. This section provides an overview of machine learning and explains how SageMaker works\. If you are a first\-time user of SageMaker, we recommend that you read the following sections in order:

1. [Machine Learning with Amazon SageMaker](how-it-works-mlconcepts.md)

1. [Explore, Analyze, and Process Data](how-it-works-notebooks-instances.md)

1. [Train a Model with Amazon SageMaker](how-it-works-training.md)

1. [Deploy a Model in Amazon SageMaker](how-it-works-deployment.md)

1. [Use Machine Learning Frameworks, Python, and R with Amazon SageMaker](frameworks.md)

1. [Get Started with Amazon SageMaker](gs.md)