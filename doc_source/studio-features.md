# Amazon SageMaker Studio features<a name="studio-features"></a>

Amazon SageMaker Studio includes the following features\.

**Topics**
+ [Amazon SageMaker Studio Notebooks](#studio-features-notebook2)
+ [Amazon SageMaker Experiments](#studio-features-experiments)
+ [Amazon SageMaker Autopilot](#studio-features-autopilot)
+ [Amazon SageMaker Debugger](#studio-features-debugger)
+ [Amazon SageMaker Model Monitor](#studio-features-model-monitor)

## Amazon SageMaker Studio Notebooks<a name="studio-features-notebook2"></a>

Amazon SageMaker Studio Notebooks is the next generation of Amazon SageMaker notebooks\. These notebooks include the following new features:
+ AWS Single Sign\-On \(AWS SSO\) integration
+ Fast start\-up times
+ Ability to share notebooks with a single click

For more information, see [Use Amazon SageMaker Studio notebooks](notebooks.md)\.

**Note**  
Because Amazon SageMaker Studio Notebooks is in preview, visual elements of Amazon SageMaker Studio might be impacted\.

## Amazon SageMaker Experiments<a name="studio-features-experiments"></a>

Amazon SageMaker provides experiment management and tracking\. Users can organize their experiments and artifacts in a centralized location using a structured organization scheme\.

An experiment is a collection of machine learning iterations called trials\. A trial is a set of steps called trial components\. A trial takes a combination of inputs such as a dataset, an algorithm, and parameters, and produces specific outputs such as a model, metrics, and checkpoints\.

Experiment tracking enables both Amazon SageMaker automated tracking of model training, tuning, and evaluation jobs, and API\-enabled tracking of experiments done locally on Amazon SageMaker notebooks\. Customers can use the tracked data to reconstruct an experiment, incrementally build on experiments conducted by peers, and trace model lineage for compliance and audit verifications\.

For more information, see [Manage Machine Learning with Amazon SageMaker Experiments](experiments.md)\.

## Amazon SageMaker Autopilot<a name="studio-features-autopilot"></a>

Amazon SageMaker Autopilot provides automatic machine learning that allows users without machine learning knowledge to quickly build classification and regression models\. Users only need to provide a tabular dataset and select the target column to predict\. Autopilot automatically explores machine learning solutions with different combinations of data preprocessors, algorithms, and algorithm parameters, to find the best model\.

When a user runs an Autopilot job, Amazon SageMaker creates an experiment for the job and then creates a trial for each combination and stores all data and results\. After the best model is determined, the user can drill down to view each trial and see which features had the most influence on the result\.

For more information, see [Use Amazon SageMaker Autopilot to Automate Model Development](autopilot-automate-model-development.md)\.

## Amazon SageMaker Debugger<a name="studio-features-debugger"></a>

Amazon SageMaker Debugger provides full visibility into the model training process by enabling the inspection of all the training parameters and data throughout the training process\.

Debugger provides a visual interface to analyze debug data and visual indicators about potential anomalies in the data\.

Debugger automatically detects and alerts users to commonly occurring errors such as parameter values getting too large or small\. Users can extend Debugger to detect new classes of errors that are specific to their model\.

For more information, see [Amazon SageMaker Debugger](train-debugger.md)\.

## Amazon SageMaker Model Monitor<a name="studio-features-model-monitor"></a>

Amazon SageMaker Model Monitor is a tool for the monitoring and analysis of models in production \(Amazon SageMaker endpoints\)\. Model Monitor offers a framework\-agnostic analysis\.

Machine learning models are typically trained and evaluated using historical data\. After they are deployed in production, the quality of their predictions can degrade over time due to model drift\. Model drift is when the distribution of the data sent to the models for predictions varies from the distribution of data used during training\.

Model Monitor continuously monitors and analyzes the prediction requests\. Model Monitor can store this data and use built\-in statistical rules to detect common issues such as outliers in data and data drift\.

For more information, see [Amazon SageMaker Model Monitor](model-monitor.md)\.