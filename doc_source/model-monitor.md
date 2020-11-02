# Amazon SageMaker Model Monitor<a name="model-monitor"></a>

Amazon SageMaker Model Monitor continuously monitors the quality of Amazon SageMaker machine learning models in production\. It enables developers to set alerts for when there are deviations in the model quality, such as data drift\. Early and pro\-active detection of these deviations enables you to take corrective actions, such as retraining models, auditing upstream systems, or fixing data quality issues without having to monitor models manually or build additional tooling\. You can use Model Monitor pre\-built monitoring capabilities that do not require coding\. You also have the flexibility to monitor models by coding to provide custom analysis\.

**Topics**
+ [How Model Monitor Works](#model-monitor-how-it-works)
+ [Capture Data](model-monitor-data-capture.md)
+ [Create a Baseline](model-monitor-create-baseline.md)
+ [Schedule Monitoring Jobs](model-monitor-scheduling.md)
+ [Interpret Results](model-monitor-interpreting-results.md)
+ [Advanced Topics](model-monitor-advanced-topics.md)

## How Model Monitor Works<a name="model-monitor-how-it-works"></a>

Amazon SageMaker Model Monitor automatically monitors machine learning \(ML\) models in production and notifies you when data quality issues arise\. ML models in production have to make predictions on real\-life data that is not carefully curated like most training datasets\. If the statistical nature of the data that your model receives while in production drifts away from the nature of the baseline data it was trained on, the model begins to lose accuracy in its predictions\. Model Monitor uses rules to detect data drift and alerts you when it happens\. The following figure shows how this process works\.

![\[The model monitoring process with Amazon SageMaker Model Monitor.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model-monitor-how-it-works-2.jpg)

To enable model monitoring, you take the following steps, which follow the path of the data through the various data collection, monitoring, and analysis processes\.
+ **[Capture Data](model-monitor-data-capture.md)**: Enable the endpoint to capture data from incoming requests to a trained ML model and the resulting model predictions\.
+ **[Create a Baseline](model-monitor-create-baseline.md)**: Create a baseline from the dataset that was used to train the model\. Compute baseline schema constraints and statistics for each feature using [Deequ](https://github.com/awslabs/deequ), an open source library built on Apache Spark, which is used to measure data quality in large datasets\.
+ **[Schedule Monitoring Jobs](model-monitor-scheduling.md)**: Create a monitoring schedule specifying what data to collect, how often to collect it, how to analyze it, and which reports to produce\. 
+ **[Interpret Results](model-monitor-interpreting-results.md)**: Inspect the reports, which compare the latest data with the baseline, and watch for any violations reported and for metrics and notifications from Amazon CloudWatch\.

**Note**  
Amazon SageMaker Model Monitor currently supports only endpoints that host a single model and does not support monitoring multi\-model endpoints\. For information on using multi\-model endpoints, see [ Host Multiple Models with Multi\-Model Endpoints](multi-model-endpoints.md) \.  
Amazon SageMaker Model Monitor does support monitoring inference pipelines, but capturing and analyzing data is done for the entire pipeline, not for individual containers in the pipeline\.

### Model Monitor Sample Notebooks<a name="model-monitor-sample-notebooks"></a>

For a sample notebook that takes you through the full end\-to\-end workflow for Model Monitor, see the [Introduction to Amazon SageMaker Model Monitor](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_model_monitor/introduction/SageMaker-ModelMonitoring.ipynb)\. 

For a sample notebook that enables the model monitoring experience for an existing endpoint, see the [Enable Model Monitoring](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_model_monitor/enable_model_monitor/SageMaker-Enable-Model-Monitor.ipynb)\. 

For a sample notebook that visualizes the statistics\.json file for a selected execution in a monitoring schedule, see the [Model Monitor Visualization](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_model_monitor/visualization/SageMaker-Model-Monitor-Visualize.ipynb)\. 

For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all the SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.