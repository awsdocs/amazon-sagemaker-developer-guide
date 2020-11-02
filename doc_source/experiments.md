# Manage Machine Learning with Amazon SageMaker Experiments<a name="experiments"></a>

Amazon SageMaker Experiments is a capability of Amazon SageMaker that lets you organize, track, compare, and evaluate your machine learning experiments\.

Machine learning is an iterative process\. You need to experiment with multiple combinations of data, algorithm and parameters, all the while observing the impact of incremental changes on model accuracy\. Over time this iterative experimentation can result in thousands of model training runs and model versions\. This makes it hard to track the best performing models and their input configurations\. It’s also difficult to compare active experiments with past experiments to identify opportunities for further incremental improvements\.

SageMaker Experiments automatically tracks the inputs, parameters, configurations, and results of your iterations as *trials*\. You can assign, group, and organize these trials into *experiments*\. SageMaker Experiments is integrated with Amazon SageMaker Studio providing a visual interface to browse your active and past experiments, compare trials on key performance metrics, and identify the best performing models\.

SageMaker Experiments comes with its own [Experiments SDK](https://github.com/aws/sagemaker-experiments) which makes the analytics capabilities easily accessible in Amazon SageMaker Notebooks\. Because SageMaker Experiments enables tracking of all the steps and artifacts that went into creating a model, you can quickly revisit the origins of a model when you are troubleshooting issues in production, or auditing your models for compliance verifications\.

**Topics**
+ [SageMaker Experiments Features](#experiments-features)
+ [Create an Amazon SageMaker Experiment](experiments-create.md)
+ [View and Compare Amazon SageMaker Experiments, Trials, and Trial Components](experiments-view-compare.md)
+ [Track and Compare Tutorial](experiments-mnist.md)
+ [Search Experiments Using Amazon SageMaker Studio](experiments-search-studio.md)
+ [Clean Up Amazon SageMaker Experiment Resources](experiments-cleanup.md)
+ [Search Using the Amazon SageMaker Console and API](search.md)

## SageMaker Experiments Features<a name="experiments-features"></a>

The following sections provide a brief overview of the features provided by SageMaker Experiments\.

**Topics**
+ [Organize Experiments](#exp-mgmt-organize)
+ [Track Experiments](#exp-mgmt-track)
+ [Compare and Evaluate Experiments](#exp-mgmt-compare)
+ [Amazon SageMaker Autopilot](#exp-mgmt-automl)

### Organize Experiments<a name="exp-mgmt-organize"></a>

Amazon SageMaker Experiments offers a structured organization scheme to help users group and organize their machine learning iterations\. The top level entity, an *experiment*, is a collection of *trials* that are observed, compared, and evaluated as a group\. A trial is a set of steps called *trial components*\. Each trial component can include a combination of inputs such as datasets, algorithms, and parameters, and produce specific outputs such as models, metrics, datasets, and checkpoints\. Examples of trial components are data pre\-processing jobs, training jobs, and batch transform jobs\.

The goal of an experiment is to determine the trial that produces the best model\. Multiple trials are performed, each one isolating and measuring the impact of a change to one or more inputs, while keeping the remaining inputs constant\. By analyzing the trials, you can determine which features have the most effect on the model\.

### Track Experiments<a name="exp-mgmt-track"></a>

Amazon SageMaker Experiments enables tracking of experiments\.

**Automated Tracking**

SageMaker Experiments automatically tracks Amazon SageMaker Autopilot jobs as experiments with their underlying training jobs tracked as trials\. SageMaker Experiments also automatically tracks SageMaker independently executed training, batch transform, and processing jobs as trial components, whether assigned to a trial or left unassigned\. Unassigned trial components can be associated with a trial at a later time\. All experiment artifacts including datasets, algorithms, hyperparameters, and model metrics are tracked and recorded\. This data allows customers to trace the complete lineage of a model which helps with model governance, auditing, and compliance verifications\.

**Manual Tracking**

SageMaker Experiments provides tracking APIs for recording and tracking machine learning workflows running locally on SageMaker Studio notebooks, including classic SageMaker notebooks\. These experiments must be part of a SageMaker training, batch transform, or processing job\.

### Compare and Evaluate Experiments<a name="exp-mgmt-compare"></a>

Amazon SageMaker Experiments is integrated with Amazon SageMaker Studio\. When you use SageMaker Studio, SageMaker Experiments automatically tracks your experiments and trials, and presents visualizations of the tracked data and an interface to search the data\.

SageMaker Experiments automatically organizes, ranks, and sorts trials based on a chosen metric using the concept of a trial leaderboard\. SageMaker Studio produces real\-time data visualizations, such as metric charts and graphs, to quickly compare and identify the best performing models\. These are updated in real\-time as the experiment progresses\.

### Amazon SageMaker Autopilot<a name="exp-mgmt-automl"></a>

Amazon SageMaker Experiments is integrated with Amazon SageMaker Autopilot\. When you perform an Autopilot job, SageMaker Experiments creates an experiment for the job, and trials for each of the different combinations of the available trial components, parameters, and artifacts\. You can visually drill into all trials and components using SageMaker Studio\.