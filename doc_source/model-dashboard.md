# SageMaker Model Dashboard<a name="model-dashboard"></a>

Amazon SageMaker Model Dashboard is a centralized portal, accessible from the SageMaker console, where you can view, search, and explore all of the models in your account\. You can track which models are deployed for inference and if they are used in batch transform jobs or hosted on endpoints\. If you set up monitors with Amazon SageMaker Model Monitor, you can also track the performance of your models as they make real\-time predictions on live data\. You can use the dashboard to find models that violate thresholds you set for data quality, model quality, bias and explainability\. The dashboard’s comprehensive presentation of all your monitor results helps you quickly identify models that don’t have these metrics configured\.

The SageMaker Model Dashboard aggregates model\-related information from several SageMaker features\. In addition to the services provided in Model Monitor, you can view model cards, visualize workflow lineage, and track your endpoint performance\. You no longer have to sort through logs, query in notebooks, or access other AWS services to collect the data you need\. With a cohesive user experience and integration into existing services, SageMaker’s SageMaker Model Dashboard provides an out\-of\-the\-box model governance solution to help you ensure quality coverage across all your models\.

**Prerequisites**

To use the SageMaker Model Dashboard, you should have one or more models in your account\. You can train models using Amazon SageMaker or import models you've trained elsewhere\. To create a model in SageMaker, you can use the `CreateModel` API\. For more information, see [CreateModel](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html)\. You can also use SageMaker\-provided ML environments, such as Amazon SageMaker Studio, which provides project templates that set up model training and deployment for you\. For information about how to get started with Studio, see [Amazon SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.htm)\.

While this is not a mandatory prerequisite, customers gain the most value out of the dashboard if they set up model monitoring jobs using SageMaker Model Monitor for models deployed to endpoints\. For prerequisites and instructions on how to use SageMaker Model Monitor, see [Monitor models for data and model quality, bias, and explainability](model-monitor.md)\.

## SageMaker Model Dashboard elements<a name="dashelem"></a>

The SageMaker Model Dashboard view extracts high\-level details from each model to provide a comprehensive summary of every model in your account\. If your model is deployed for inference, the dashboard helps you track the performance of your model and endpoint in real time\.

Important details to highlight in this page include:
+ **Risk rating**: A user\-specified parameter from the model card with a **low**, **medium**, or **high** value\. The model card’s risk rating is a categorical measure of the business impact of the model’s predictions\. Models are used for a variety of business applications, each of which assumes a different level of risk\. For example, incorrectly detecting a cyber attack has much greater business impact than incorrectly categorizing an email\. If you don’t know the model risk, you can set it to **unknown**\. For information about Amazon SageMaker Model Cards see [Model Cards](https://docs.aws.amazon.com/sagemaker/latest/dg/model-cards.html)\.
+ Model Monitor alerts: Model Monitor alerts are a primary focus of the SageMaker Model Dashboard, and reviewing the existing documentation on the various monitors provided by SageMaker is a helpful way to get started\. For an in\-depth explanation of the SageMaker Model Monitor feature and sample notebooks, see [Monitor models for data and model quality, bias, and explainability](model-monitor.md)\.

  The SageMaker Model Dashboard displays Model Monitor status values by the following monitor types:
  + *Data Quality*: Compares live data to training data\. If they diverge, your model's inferences may no longer be accurate\. For additional details about the Data Quality monitor, see [Monitor data quality](model-monitor-data-quality.md)\.
  + *Model Quality*: Compares the predictions that the model makes with the actual Ground Truth labels that the model attempts to predict\. For additional details about the Model Quality monitor, see [Monitor model quality](model-monitor-model-quality.md)\.
  + *Bias Drift*: Compares the distribution of live data to training data, which can also cause inaccurate predictions\. For additional details about the Bias Drift monitor, see [Monitor Bias Drift for Models in Production](clarify-model-monitor-bias-drift.md)\.
  + *Feature Attribution Drift*: Also known as explainability drift\. Compares the relative rankings of your features in training data versus live data, which could also be a result of bias drift\. For additional details about the Feature Attribution Drift monitor, see [Monitor Feature Attribution Drift for Models in Production](clarify-model-monitor-feature-attribution-drift.md)\.

  Each Model Monitor status is one of the following values:
  + **None**: No monitor is scheduled
  + **Inactive**: A monitor was scheduled, but it was deactivated
  + **OK**: A monitor is scheduled and is active, and has not encountered the necessary number of violations in recent model monitor executions to raise an alert
  + Time and date: An active monitor raised an alert at the specified time and date
+ **Endpoint**: The endpoints which host your model for real\-time inference\. Within the SageMaker Model Dashboard, you can select the endpoint column to view performance metrics such as CPU, GPU, disk, and memory utilization of your endpoints in real time to help you track the performance of your compute instances\.
+ **Batch transform job**: The most recent batch transform job that ran using this model\. This column helps you determine if a model is actively used for batch inference\.
+ Model details: Each entry in the dashboard links to a model details page where you can dive deeper into an individual model\. You can access the model’s lineage graph, which visualizes the workflow from data preparation to deployment, and metadata for each step\. You can also create and view the model card, review alert details and history, assess the performance of your real\-time endpoints, and access other infrastructure\-related details\.