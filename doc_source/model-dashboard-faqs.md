# SageMaker Model Dashboard FAQ<a name="model-dashboard-faqs"></a>

Refer to the following FAQ topics for answers to commonly asked questions about Amazon SageMaker Model Dashboard\.

## Q\. What is SageMaker Model Dashboard?<a name="model-dashboard-faqs-whatis"></a>

Amazon SageMaker Model Dashboard is a centralized repository of all models created in your account\. The models are generally the outputs of SageMaker training jobs, but you can also import models trained elsewhere and host them on SageMaker\. SageMaker Model Dashboard provides a single interface for IT administrators, model risk managers, and business leaders to track all deployed models and aggregates data from multiple AWS services to provide indicators about how your models are performing\. You can view details about model endpoints, batch transform jobs, and monitoring jobs for additional insights into model performance\. The dashboard’s visual display helps you quickly identify which models have missing or inactive monitors so you can ensure all models are periodically checked for data drift, model drift, bias drift, and feature attribution drift\. Lastly, the dashboard’s ready access to model details helps you dive deep so you can access logs, infrastructure\-related information, and resources to help you debug monitoring failures\.

## Q\. What are the prerequisites to use SageMaker Model Dashboard?<a name="model-dashboard-faqs-access"></a>

You should have one or more models created in SageMaker, either trained on SageMaker or externally trained\. While this is not a mandatory prerequisite, you gain the most value from the dashboard if you set up model monitoring jobs via Amazon SageMaker Model Monitor for models deployed to endpoints\.

## Q\. Who should use SageMaker Model Dashboard?<a name="model-dashboard-faqs-users"></a>

Model risk managers, ML practitioners, data scientists and business leaders can get a comprehensive overview of models using the SageMaker Model Dashboard\. The dashboard aggregates and displays data from Amazon SageMaker Model Cards, Endpoints and Model Monitor services to display valuable information such as model metadata from the model card and model registry, endpoints where the models are deployed, and insights from model monitoring\.

## Q\. How do I use SageMaker Model Dashboard?<a name="model-dashboard-faqs-how"></a>

SageMaker Model Dashboard is available out of the box with Amazon SageMaker and does not require any prior configuration\. However, if you have set up model monitoring jobs using SageMaker Model Monitor and Clarify, you use Amazon CloudWatch to configure alerts that raise a flag in the dashboard when model performance deviates from an acceptable range\. You can create and add new model cards to the dashboard, and view all the monitoring results associated with endpoints\. SageMaker Model Dashboard currently does not support cross\-account models\.

## Q\. What is Amazon SageMaker Model Monitor?<a name="model-dashboard-faqs-what"></a>

With Amazon SageMaker Model Monitor, you can select the data you want to monitor and analyze without writing any code\. SageMaker Model Monitor lets you select data, such as prediction output, from a menu of options and captures metadata such as timestamp, model name, and endpoint so you can analyze model predictions\. You can specify the sampling rate of data capture as a percentage of overall traffic in the case of high volume real\-time predictions\. This data is stored in your own Amazon S3 bucket\. You can also encrypt this data, configure fine\-grained security, define data retention policies, and implement access control mechanisms for secure access\.

## Q\. What types of model monitors does SageMaker support?<a name="model-dashboard-faqs-types"></a>

SageMaker Model Monitor provides the following types of [model monitors](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html):
+ *Data Quality*: Monitor drift in data quality\.
+ *Model Quality*: Monitor drift in model quality metrics, such as accuracy\.
+ *Bias Drift for Models in Production*: Monitor bias in your model's predictions by comparing the distribution of training and live data\.
+ *Feature Attribution Drift for Models in Production*: Monitor drift in feature attribution by comparing the relative rankings of features in training and live data\.

## Q\. What inference methods does SageMaker Model Monitor support?<a name="model-dashboard-faqs-inference"></a>

Model Monitor currently supports endpoints that host a single model for real\-time inference or batch transform, and does not support monitoring of [multi\-model endpoints](https://docs.aws.amazon.com/sagemaker/latest/dg/multi-model-endpoints.html)\.

## Q\. How can I get started with SageMaker Model Monitor?<a name="model-dashboard-faqs-get-started"></a>

You can use the following resources to get started with model monitoring:
+ [Data quality monitor example notebook](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker_model_monitor/introduction/SageMaker-ModelMonitoring.ipynb)
+ [Model quality monitor example notebook](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker_model_monitor/introduction/SageMaker-ModelMonitoring.ipynb)
+ [Bias drift monitor example notebook](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker_model_monitor/fairness_and_explainability/SageMaker-Model-Monitor-Fairness-and-Explainability.ipynb)
+ [Feature attribution drift monitor example notebook](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker_model_monitor/fairness_and_explainability/SageMaker-Model-Monitor-Fairness-and-Explainability.ipynb)

For more examples of model monitoring, see the GitHub repository [amazon\-sagemaker\-examples](https://github.com/aws/amazon-sagemaker-examples/tree/main/sagemaker_model_monitor)\.

## Q\. How does Model Monitor work?<a name="model-dashboard-faqs-mm-work"></a>

Amazon SageMaker Model Monitor automatically monitors machine learning models in production, using rules to detect drift in your model\. Model Monitor notifies you when quality issues arise through alerts\. To learn more, see [How Model Monitor Works](model-monitor.md#model-monitor-how-it-works)\.

## Q\. When and how do you bring your own container \(BYOC\) for Model Monitor?<a name="model-dashboard-faqs-byoc-when-how"></a>

Model Monitor computes model metrics and statistics on tabular data only\. For use cases other than tabular datasets, such as images or text, you can bring your own containers \(BYOC\) to monitor your data and models\. For example, you can use BYOC to monitor an image classification model that takes images as input and outputs a label\. To learn more about container contracts, see [Bring Your Own Containers](model-monitor-byoc-containers.md)\.

## Q\. Where can I find examples of BYOC for Model Monitor?<a name="model-dashboard-faqs-byoc-examples"></a>

You can find helpful BYOC examples in the following links:
+ [Monitor models for data and model quality, bias, and explainability](model-monitor.md)
+ [GitHub example repository](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker_model_monitor)
+ [Bring Your Own Containers](model-monitor-byoc-containers.md)
+ [Detecting data drift in NLP using BYOC Model Monitor](http://aws.amazon.com/blogs/machine-learning/detect-nlp-data-drift-using-custom-amazon-sagemaker-model-monitor)
+ [ Detecting and analyzing incorrect predictions in CV ](http://aws.amazon.com/blogs/machine-learning/detecting-and-analyzing-incorrect-model-predictions-with-amazon-sagemaker-model-monitor-and-debugger)

## Q\. How do I integrate Model Monitor with SageMaker Pipelines?<a name="model-dashboard-integrate-mm-pipelines"></a>

For details about how to integrate Model Monitor and SageMaker Pipelines, see [ Amazon SageMaker Pipelines now integrates with SageMaker Model Monitor and SageMaker Clarify ](https://aws.amazon.com/about-aws/whats-new/2021/12/amazon-sagemaker-pipelines-integrates-sagemaker-model-monitor-sagemaker-clarify/)\.

For an example, see the GitHub sample notebook [SageMaker Pipelines integration with Model Monitor and Clarify](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-pipelines/tabular/model-monitor-clarify-pipelines/sagemaker-pipeline-model-monitor-clarify-steps.ipynb)\.

## Q\. Are there any performance concerns using `DataCapture`?<a name="model-dashboard-datacapture"></a>

When turned on, data capture occurs asynchronously on the SageMaker endpoints\. To prevent impact to inference requests, `DataCapture` stops capturing requests at high levels of disk usage\. It is recommended you keep your disk utilization below 75% to ensure `DataCapture` continues capturing requests\.