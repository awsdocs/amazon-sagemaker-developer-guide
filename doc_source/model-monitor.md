# Monitor models for data and model quality, bias, and explainability<a name="model-monitor"></a>

Amazon SageMaker Model Monitor continuously monitors the quality of Amazon SageMaker machine learning models in production\. With Model Monitor, you can set alerts that notify you when there are deviations in the model quality\. Early and proactive detection of these deviations enables you to take corrective actions, such as retraining models, auditing upstream systems, or fixing quality issues without having to monitor models manually or build additional tooling\. You can use Model Monitor prebuilt monitoring capabilities that do not require coding\. You also have the flexibility to monitor models by coding to provide custom analysis\.

Model Monitor provides the following types of monitoring:
+ [Monitor data quality](model-monitor-data-quality.md) \- Monitor drift in data quality\.
+ [Monitor model quality](model-monitor-model-quality.md) \- Monitor drift in model quality metrics, such as accuracy\.
+ [Monitor Bias Drift for Models in Production](clarify-model-monitor-bias-drift.md) \- Monitor bias in you model's predictions\.
+ [Monitor Feature Attribution Drift for Models in Production](clarify-model-monitor-feature-attribution-drift.md) \- Monitor drift in feature attribution\.

**Topics**
+ [How Model Monitor Works](#model-monitor-how-it-works)
+ [Monitor data quality](model-monitor-data-quality.md)
+ [Monitor model quality](model-monitor-model-quality.md)
+ [Monitor Bias Drift for Models in Production](clarify-model-monitor-bias-drift.md)
+ [Monitor Feature Attribution Drift for Models in Production](clarify-model-monitor-feature-attribution-drift.md)
+ [Capture data](model-monitor-data-capture.md)
+ [Schedule monitoring jobs](model-monitor-scheduling.md)
+ [Amazon SageMaker Model Monitor prebuilt container](model-monitor-pre-built-container.md)
+ [Interpret results](model-monitor-interpreting-results.md)
+ [Visualize results in Amazon SageMaker Studio](model-monitor-interpreting-visualize-results.md)
+ [Advanced topics](model-monitor-advanced-topics.md)

## How Model Monitor Works<a name="model-monitor-how-it-works"></a>

Amazon SageMaker Model Monitor automatically monitors machine learning \(ML\) models in production and notifies you when quality issues arise\. Model Monitor uses rules to detect drift in your models and alerts you when it happens\. The following figure shows how this process works\.

![\[The model monitoring process with Amazon SageMaker Model Monitor.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mmv2-architecture.png)

To enable model monitoring, you take the following steps, which follow the path of the data through the various data collection, monitoring, and analysis processes:
+ Enable the endpoint to capture data from incoming requests to a trained ML model and the resulting model predictions\.
+ Create a baseline from the dataset that was used to train the model\. The baseline computes metrics and suggests constraints for the metrics\. Real\-time predictions from your model are compared to the constraints, and are reported as violations if they are outside the constrained values\.
+ Create a monitoring schedule specifying what data to collect, how often to collect it, how to analyze it, and which reports to produce\. 
+ Inspect the reports, which compare the latest data with the baseline, and watch for any violations reported and for metrics and notifications from Amazon CloudWatch\.

**Notes**  
Model Monitor currently supports only tabular data\.
Model Monitor currently supports only endpoints that host a single model and does not support monitoring multi\-model endpoints\. For information on using multi\-model endpoints, see [Host multiple models in one container behind one endpoint](multi-model-endpoints.md)\.
Model Monitor supports monitoring inference pipelines, but capturing and analyzing data is done for the entire pipeline, not for individual containers in the pipeline\.
To prevent impact to inference requests, Data Capture stops capturing requests at high levels of disk usage\. It is recommended you keep your disk utilization below 75% in order to ensure data capture continues capturing requests\.
If you launch SageMaker Studio in a custom Amazon VPC, you need to create VPC endpoints to enable Model Monitor to communicate with Amazon S3 and CloudWatch\. For information about VPC endpoints, see [VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) in the *Amazon Virtual Private Cloud User Guide*\. For information about launching SageMaker Studio in a custom VPC, see [Connect SageMaker Studio Notebooks to Resources in a VPC](studio-notebooks-and-internet-access.md)\.

### Model Monitor Sample Notebooks<a name="model-monitor-sample-notebooks"></a>

For a sample notebook that takes you through the full end\-to\-end workflow for Model Monitor, see the [Introduction to Amazon SageMaker Model Monitor](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_model_monitor/introduction/SageMaker-ModelMonitoring.html)\. 

For a sample notebook that visualizes the statistics\.json file for a selected execution in a monitoring schedule, see the [Model Monitor Visualization](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_model_monitor/visualization/SageMaker-Model-Monitor-Visualize.html)\. 

For instructions that show you how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all the SageMaker samples\. To open a notebook, choose the notebook's **Use** tab and choose **Create copy**\.