# Use Amazon SageMaker Autopilot to automate model development<a name="autopilot-automate-model-development"></a>

Amazon SageMaker Autopilot simplifies the machine learning experience by automating machine learning processes\. It helps you explore your data, engineer features, try different algorithms, and select the best model\. The result is the best performing model that you can deploy at a fraction of the time normally required\. 

You also get full visibility into how the data was wrangled and how the model was created and selected, based on performance, from the various candidates explored\. This is provided by notebooks that Autopilot generates that describe the plan it followed when selecting candidate models from trials combining data processing and algorithm training and tuning\. The notebooks also provide educational tools that enable you to conduct ML experiments\. You can learn about the impact of various inputs and trade\-offs made in experiments first by examining the data exploration and candidate generation notebooks exposed by Autopilot and then by making your own modifications and rerunning the notebooks\.

The following graphic shows how Autopilot manages the principal tasks of the machine learning process\.

![\[Overview of the process used by Amazon SageMaker Autopilot.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Autopilot-process-graphic-1.png)

You can use Autopilot in different ways: on autopilot \(hence the name\) or with various degrees of human guidance, without code through Amazon SageMaker Studio, or with code using one of the AWS SDKs\. Autopilot currently supports regression and binary and multiclass classification\. It also only supports tabular data formatted in files with comma\-separated values\.

**Topics**
+ [Get started with Amazon SageMaker Autopilot](autopilot-automate-model-development-get-started.md)
+ [Amazon SageMaker Autopilot problem types](autopilot-automate-model-development-problem-types.md)
+ [Create an Amazon SageMaker experiment in SageMaker Studio](autopilot-automate-model-development-create-experiment.md)
+ [Amazon SageMaker Autopilot notebook output](autopilot-automate-model-development-notebook-output.md)
+ [API reference guide for Amazon SageMaker Autopilot](autopilot-reference.md)