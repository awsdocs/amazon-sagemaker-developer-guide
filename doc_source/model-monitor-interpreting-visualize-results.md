# Visualize results for real\-time endpoints in Amazon SageMaker Studio<a name="model-monitor-interpreting-visualize-results"></a>

If you are monitoring a real\-time endpoint, you can also visualize the results in Amazon SageMaker Studio\. You can view the details of any monitoring job run, and you can create charts that show the baseline and captured values for any metric that the monitoring job calculates\.

**To view the detailed results of a monitoring job**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the left navigation pane, choose the **Components and registries** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Components_registries.png) \)\.

1. Choose **Endpoints** in the drop\-down menu\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mm-studio-endpoints.png)

1. On the endpoint tab, choose the monitoring type for which you want to see job details\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mm-studio-model-quality.png)

1. Choose the name of the monitoring job run for which you want to view details from the list of monitoring jobs\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mm-studio-job-history.png)

1. The **MONITORING JOB DETAILS** tab opens with a detailed report of the monitoring job\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mm-studio-job-details.png)

You can create a chart that displays the baseline and captured metrics for a time period\.

**To create a chart in SageMaker Studio to visualize monitoring results**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the left navigation pane, choose the **Components and registries** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Components_registries.png) \)\.

1. Choose **Endpoints** in the drop\-down menu\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mm-studio-endpoints.png)

1. On the **Endpoint** tab, choose the monitoring type you want to create a chart for\. This example shows a chart for the **Model quality** monitoring type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mm-studio-model-quality.png)

1. Choose **Add chart**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mm-studio-add-chart.png)

1. On the **CHART PROPERTIES** tab, choose the time period, statistic, and metric that you want to chart\. This example shows a chart for a **Timeline** of **1 week**, the **Average** **Statistic** of, and the **F1** **Metric**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mm-studio-chart-properties.png)

1. The chart that shows the baseline and current metric statistic you chose in the previous step shows up in the **Endpoint** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_monitor/mm-studio-f1-chart.png)