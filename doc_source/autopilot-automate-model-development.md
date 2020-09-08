# Automate model development with Amazon SageMaker Autopilot<a name="autopilot-automate-model-development"></a>

Amazon SageMaker Autopilot is a feature\-set that automates key tasks of an automatic machine learning \(AutoML\) process\. It explores your data, selects the algorithms relevant to your problem type, and prepares the data to facilitate model training and tuning\. It simplifies your machine learning experience by automating these key tasks that constitute an AutoML process\. It ranks all of the optimized models tested by their performance\. It finds the best performing model that you can deploy at a fraction of the time normally required\. 

You get full visibility into how the data was wrangled and how the models were selected, trained and tuned for each of the candidates tested\. This is provided by notebooks that Autopilot generates for each trial that contain the code used to explore the data and find the best candidates\. The notebooks also provide educational tools that enable you to learn about and conduct your own ML experiments\. You can learn about the impact of various inputs and trade\-offs made in experiments by examining the various data exploration and candidate generation notebooks exposed by Autopilot\. You can also conduct further experiments on the higher performing candidates by making your own modifications to the notebooks and rerunning them\.

The following graphic outlines the principal tasks of an AutoML process managed by Autopilot\.

![\[Overview of the AutoML process used by Amazon SageMaker Autopilot.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Autopilot-process-graphic-1.png)

You can use Autopilot in different ways: on autopilot \(hence the name\) or with various degrees of human guidance, without code through Amazon SageMaker Studio, or with code using one of the AWS SDKs\. Autopilot currently supports regression and binary and multiclass classification\. It also only supports tabular data formatted in files with comma\-separated values\.

With Amazon SageMaker, you pay only for what you use\. Building, training, and deploying ML models is billed by the second, with no minimum fees and no upfront commitments\. For more information about the cost of using SageMaker, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing)\.

**Topics**
+ [Get started with Amazon SageMaker Autopilot](autopilot-automate-model-development-get-started.md)
+ [Create an Amazon SageMaker Autopilot experiment](autopilot-automate-model-development-create-experiment.md)
+ [Amazon SageMaker Autopilot problem types](autopilot-automate-model-development-problem-types.md)
+ [Amazon SageMaker Autopilot notebook output](autopilot-automate-model-development-notebook-output.md)
+ [Amazon SageMaker Autopilot container output](autopilot-automate-model-development-container-output.md)
+ [API reference guide for Amazon SageMaker Autopilot](autopilot-reference.md)