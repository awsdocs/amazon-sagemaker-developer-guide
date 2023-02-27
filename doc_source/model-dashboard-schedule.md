# View Model Monitor schedules and alerts<a name="model-dashboard-schedule"></a>

Using the Python SDK, you can create a model monitor for data quality, model quality, bias drift, or feature attribution drift\. For more information about using SageMaker Model Monitor, see [Monitor models for data and model quality, bias, and explainability](model-monitor.md)\. The SageMaker Model Dashboard populates information from all the monitors you create on all your models in your account\. You can track the status of each monitor, which indicates whether your monitor is running as expected or failed due to an internal error\. You can also activate or deactivate any monitor in the model details page itself\. For instructions about how to view scheduled monitors for a model, see [View scheduled monitors](#model-dashboard-schedule-view)\. For instructions about how to activate or deactivate model monitors, see [Activate or deactivate a model monitor](#model-dashboard-schedule-activate)\.

A properly\-configured and actively\-running model monitor might raise alerts, in which case the monitoring executions produce violation reports\. For details about how alerts work and how to view alert results, history, and links to job reports for debug, see [View and edit alerts](model-dashboard-alerts.md)\.

## View scheduled monitors<a name="model-dashboard-schedule-view"></a>

**To view a model’s scheduled monitors, complete the following steps:**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Governance** in the left panel\.

1. Choose **SageMaker Model Dashboard**\.

1. In the **Models** section of the SageMaker Model Dashboard, select the model name of the scheduled monitors you want to view\.

1. View the scheduled monitors in the **Monitor schedule** section\. You can review the status for each monitor in the **Status schedule** column, which is one of the following values:
   + **Failed**: The monitoring schedule failed due to a problem with the configuration or settings \(such as incorrect user permissions\)\.
   + **Pending**: The monitor is in the process of becoming scheduled\.
   + **Stopped**: The schedule is stopped by the user\.
   + **Scheduled**: The schedule is created and runs at the frequency you specified\.

## Activate or deactivate a model monitor<a name="model-dashboard-schedule-activate"></a>

**To activate or deactivate a model monitor, complete the following steps:**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Governance** in the left panel\.

1. Choose **SageMaker Model Dashboard**\.

1. In the **Models** section of the SageMaker Model Dashboard, select the model name of the alert you want to modify\.

1. Choose the radio box next to the monitor schedule of the alert you want to modify\.

1. \(optional\) Choose **Deactivate monitor schedule** if you want to deactivate your monitor schedule\.

1. \(optional\) Choose **Activate monitor schedule** if you want to activate your monitor schedule\.