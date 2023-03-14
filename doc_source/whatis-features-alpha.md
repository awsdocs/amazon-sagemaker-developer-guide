# Amazon SageMaker Features<a name="whatis-features-alpha"></a>

Amazon SageMaker includes the following features\.

**Topics**
+ [New features for re:Invent 2022](#whatis-features-alpha-new)
+ [Machine learning environments](#whatis-features-alpha-mle)
+ [Major features](#whatis-features-alpha-major)

## New features for re:Invent 2022<a name="whatis-features-alpha-new"></a>

SageMaker includes the following new features for re:Invent 2022\.

**[SageMaker geospatial capabilities](geospatial.md)**  
Build, train, and deploy ML models using geospatial data\.

**[SageMaker Model Cards](model-cards.md)**  
Document information about your ML models in a single place for streamlined governance and reporting throughout the ML lifecycle\.

**[SageMaker Model Dashboard](model-dashboard.md)**  
A pre\-built, visual overview of all the models in your account\. Model Dashboard integrates information from SageMaker Model Monitor, transform jobs, endpoints, lineage tracking, and CloudWatch so you can access high\-level model information and track model performance in one unified view\.

**[SageMaker Role Manager](role-manager.md)**  
Administrators can define least\-privilege permissions for common ML activities using custom and preconfigured persona\-based IAM roles\.

**[AutoML step](build-and-manage-steps.md)**  
Create an AutoML job to automatically train a model in SageMaker Pipelines\.

**[Collaboration with shared spaces](domain-space.md)**  
A shared space consists of a shared JupyterServer application and a shared directory\. All user profiles in a Domain have access to all shared spaces in the Domain\.

**[Data Wrangler data preparation widget](data-wrangler-interactively-prepare-data-notebook.md)**  
Interact with your data, get visualizations, explore actionable insights, and fix data quality issues\. 

**[Inference shadow tests](shadow-tests.md)**  
Evaluate any changes to your model\-serving infrastructure by comparing it's performance against the currently deployed infrastructure\.

**[Notebook\-based Workflows](notebook-auto-run.md)**  
Run your SageMaker Studio notebook as a non\-interactive, scheduled job\.

**[Studio Git extension](studio-git-attach.md)**  
A Git extension to enter the URL of a Git repository, clone it into your environment, push changes, and view commit history\.

## Machine learning environments<a name="whatis-features-alpha-mle"></a>

SageMaker includes the following machine learning environments\.

**[SageMaker Studio](studio.md)**  
An integrated machine learning environment where you can build, train, deploy, and analyze your models all in the same application\.

**[SageMaker Studio Lab](studio-lab.md)**  
A free service that gives customers access to AWS compute resources in an environment based on open\-source JupyterLab\.

**[SageMaker Canvas](canvas.md)**  
An auto ML service that gives people with no coding experience the ability to build models and make predictions with them\.

**[RStudio on Amazon SageMaker](rstudio.md)**  
An integrated development environment for R, with a console, syntax\-highlighting editor that supports direct code execution, and tools for plotting, history, debugging and workspace management\.

## Major features<a name="whatis-features-alpha-major"></a>

SageMaker includes the following major features in alphabetical order excluding any SageMaker prefix\.

**[Amazon Augmented AI](a2i-use-augmented-ai-a2i-human-review-loops.md)**  
Build the workflows required for human review of ML predictions\. Amazon A2I brings human review to all developers, removing the undifferentiated heavy lifting associated with building human review systems or managing large numbers of human reviewers\.

**[SageMaker Autopilot](autopilot-automate-model-development.md)**  
Users without machine learning knowledge can quickly build classification and regression models\.

**[Batch Transform](batch-transform.md)**  
Preprocess datasets, run inference when you don't need a persistent endpoint, and associate input records with inferences to assist the interpretation of results\.

**[SageMaker Clarify](clarify-fairness-and-explainability.md)**  
Improve your machine learning models by detecting potential bias and help explain the predictions that models make\.

**[SageMaker Data Wrangler](data-wrangler.md)**  
Import, analyze, prepare, and featurize data in SageMaker Studio\. You can integrate Data Wrangler into your machine learning workflows to simplify and streamline data pre\-processing and feature engineering using little to no coding\. You can also add your own Python scripts and transformations to customize your data prep workflow\.

**[SageMaker Debugger](train-debugger.md)**  
Inspect training parameters and data throughout the training process\. Automatically detect and alert users to commonly occurring errors such as parameter values getting too large or small\.

**[SageMaker Edge Manager](edge.md)**  
Optimize custom models for edge devices, create and manage fleets and run models with an efficient runtime\.

**[SageMaker Elastic Inference](ei.md)**  
Speed up the throughput and decrease the latency of getting real\-time inferences\.

**[SageMaker Experiments](experiments.md)**  
Experiment management and tracking\. You can use the tracked data to reconstruct an experiment, incrementally build on experiments conducted by peers, and trace model lineage for compliance and audit verifications\.

**[SageMaker Feature Store](feature-store.md)**  
A centralized store for features and associated metadata so features can be easily discovered and reused\. You can create two types of stores, an Online or Offline store\. The Online Store can be used for low latency, real\-time inference use cases and the Offline Store can be used for training and batch inference\.

**[SageMaker Ground Truth](sms.md)**  
High\-quality training datasets by using workers along with machine learning to create labeled datasets\.

**[SageMaker Ground Truth Plus](gtp.md)**  
A turnkey data labeling feature to create high\-quality training datasets without having to build labeling applications and manage the labeling workforce on your own\.

**[SageMaker Inference Recommender](inference-recommender.md)**  
Get recommendations on inference instance types and configurations \(e\.g\. instance count, container parameters and model optimizations\) to use your ML models and workloads\.

**[SageMaker JumpStart](studio-jumpstart.md)**  
Learn about SageMaker features and capabilities through curated 1\-click solutions, example notebooks, and pretrained models that you can deploy\. You can also fine\-tune the models and deploy them\.

**[SageMaker ML Lineage Tracking](lineage-tracking.md)**  
Track the lineage of machine learning workflows\.

**[SageMaker Model Building Pipelines](pipelines.md)**  
Create and manage machine learning pipelines integrated directly with SageMaker jobs\.

**[SageMaker Model Monitor](model-monitor.md)**  
Monitor and analyze models in production \(endpoints\) to detect data drift and deviations in model quality\.

**[SageMaker Model Registry](model-registry.md)**  
Versioning, artifact and lineage tracking, approval workflow, and cross account support for deployment of your machine learning models\.

**[SageMaker Neo](neo.md)**  
Train machine learning models once, then run anywhere in the cloud and at the edge\.

**[Preprocessing](processing-job.md)**  
Analyze and preprocess data, tackle feature engineering, and evaluate models\.

**[SageMaker Projects](sagemaker-projects.md)**  
Create end\-to\-end ML solutions with CI/CD by using SageMaker projects\.

**[Reinforcement Learning](reinforcement-learning.md)**  
Maximize the long\-term reward that an agent receives as a result of its actions\.

**[SageMaker Serverless Endpoints](serverless-endpoints.md)**  
A serverless endpoint option for hosting your ML model\. Automatically scales in capacity to serve your endpoint traffic\. Removes the need to select instance types or manage scaling policies on an endpoint\.

**[SageMaker Studio Notebooks](notebooks.md)**  
The next generation of SageMaker notebooks that include AWS IAM Identity Center \(successor to AWS Single Sign\-On\) \(IAM Identity Center\) integration, fast start\-up times, and single\-click sharing\.

**[SageMaker Studio Notebooks and Amazon EMR](studio-notebooks-emr-cluster.md)**  
Easily discover, connect to, create, terminate and manage Amazon EMR clusters in single account and cross account configurations directly from SageMaker Studio\.

**[SageMaker Training Compiler](training-compiler.md)**  
Train deep learning models faster on scalable GPU instances managed by SageMaker\.