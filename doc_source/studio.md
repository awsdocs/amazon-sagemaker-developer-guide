# Amazon SageMaker Studio<a name="studio"></a>

SageMaker Studio is a web\-based, integrated development environment \(IDE\) for machine learning that lets you build, train, debug, deploy, and monitor your machine learning models\. Studio provides all the tools you need to take your models from experimentation to production while boosting your productivity\. In a single unified visual interface, customers can perform the following tasks:
+ Write and execute code in Jupyter notebooks
+ Build and train machine learning models
+ Deploy the models and monitor the performance of their predictions
+ Track and debug the machine learning experiments

For information on the onboarding steps to sign in to SageMaker Studio, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

The following sections provide an overview of the user interface and a description of Studio's main features\.

**Topics**
+ [SageMaker Studio Region Availability](#studio-regions)
+ [SageMaker Studio Features](#studio-features)
+ [Amazon SageMaker Studio UI Overview](studio-ui.md)
+ [Use the Amazon SageMaker Studio Launcher](studio-launcher.md)
+ [Use Amazon SageMaker Studio Notebooks](notebooks.md)
+ [Perform Common Tasks in Amazon SageMaker Studio](studio-tasks.md)
+ [Amazon SageMaker Studio Pricing](studio-pricing.md)

## SageMaker Studio Region Availability<a name="studio-regions"></a>

SageMaker Studio is available in the following AWS Regions:
+ US East \(Ohio\), us\-east\-2
+ US East \(N\. Virginia\), us\-east\-1
+ US West \(N\. Oregon\), us\-west\-2
+ China \(Beijing\), cn\-north\-1
+ China \(Ningxia\), cn\-northwest\-1
+ EU \(Ireland\), eu\-west\-1

## SageMaker Studio Features<a name="studio-features"></a>

The following sections provide a brief overview of the features integrated with SageMaker Studio\. A link to more information about each feature is included after the overview\.

**Topics**
+ [Amazon SageMaker Studio Notebooks](#studio-features-notebooks)
+ [Amazon SageMaker Studio Experiments](#studio-features-experiments)
+ [Amazon SageMaker Autopilot](#studio-features-autopilot)
+ [Amazon SageMaker Debugger](#studio-features-debugger)
+ [Amazon SageMaker Model Monitor](#studio-features-model-monitor)

### Amazon SageMaker Studio Notebooks<a name="studio-features-notebooks"></a>

SageMaker Studio Notebooks is the next generation of SageMaker notebooks\. These notebooks include the following new features:
+ AWS Single Sign\-On \(AWS SSO\) integration
+ Fast start\-up times
+ Ability to share notebooks with a few clicks

For a comparison with SageMaker notebook instances, see [How Are Amazon SageMaker Studio Notebooks Different from Notebook Instances?](notebooks-comparison.md)

For more information, see [Use Amazon SageMaker Studio Notebooks](notebooks.md)\.

### Amazon SageMaker Studio Experiments<a name="studio-features-experiments"></a>

SageMaker Experiments provides experiment management and tracking\. Users can organize their experiments and artifacts in a centralized location using a structured organization scheme\.

An experiment is a collection of machine learning iterations called trials\. A trial is a set of steps called trial components\. A trial takes a combination of inputs such as a dataset, an algorithm, and parameters, and produces specific outputs such as a model, metrics, and checkpoints\.

SageMaker Experiment tracking enables both SageMaker automated tracking of model training, tuning, and evaluation jobs, and API\-enabled tracking of experiments done locally on SageMaker Studio notebooks\. Customers can use the tracked data to reconstruct an experiment, incrementally build on experiments conducted by peers, and trace model lineage for compliance and audit verifications\.

For more information, see [Manage Machine Learning with Amazon SageMaker Experiments](experiments.md)\.

### Amazon SageMaker Autopilot<a name="studio-features-autopilot"></a>

Amazon SageMaker Autopilot provides automatic machine learning that allows users without machine learning knowledge to quickly build classification and regression models\. Users only need to provide a tabular dataset and select the target column to predict\. SageMaker Autopilot automatically explores machine learning solutions with different combinations of data preprocessors, algorithms, and algorithm parameters, to find the best model\.

When a user runs an SageMaker Autopilot job, SageMaker Studio creates an experiment for the job and then creates a trial for each combination and stores all data and results\. After the best model is determined, the user can drill down to view each trial and see which features had the most influence on the result\.

For more information, see [Automate model development with Amazon SageMaker Autopilot](autopilot-automate-model-development.md)\.

### Amazon SageMaker Debugger<a name="studio-features-debugger"></a>

Amazon SageMaker Debugger provides full visibility into the model training process by enabling the inspection of all the training parameters and data throughout the training process\.

SageMaker Debugger provides a visual interface to analyze debug data and visual indicators about potential anomalies in the data\.

SageMaker Debugger automatically detects and alerts users to commonly occurring errors such as parameter values getting too large or small\. Users can extend Debugger to detect new classes of errors that are specific to their model\.

For more information, see [Amazon SageMaker Debugger](train-debugger.md)\.

### Amazon SageMaker Model Monitor<a name="studio-features-model-monitor"></a>

Amazon SageMaker Model Monitor is a tool for the monitoring and analysis of models in production \(Amazon SageMaker endpoints\)\. Model Monitor offers a framework\-agnostic analysis\.

Machine learning models are typically trained and evaluated using historical data\. After they are deployed in production, the quality of their predictions can degrade over time due to model drift\. Model drift is when the distribution of the data sent to the models for predictions varies from the distribution of data used during training\.

SageMaker Model Monitor continuously monitors and analyzes the prediction requests\. Model Monitor can store this data and use built\-in statistical rules to detect common issues such as outliers in data and data drift\.

For more information, see [Amazon SageMaker Model Monitor](model-monitor.md)\.