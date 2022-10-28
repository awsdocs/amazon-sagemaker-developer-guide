# Schedule Bias Drift Monitoring Jobs<a name="clarify-model-monitor-bias-drift-schedule"></a>

After you create your baseline, you can call the `create_monitoring_schedule()` method of your `ModelBiasModelMonitor` class instance to schedule an hourly bias drift monitor\. The following sections show you how to create bias drift monitor for a model deployed to a real\-time endpoint as well as for a batch transform job\.

Unlike data quality monitoring, you need to supply Ground Truth labels if you want to monitor model quality\. However, Ground Truth labels could be delayed\. To address this, specify offsets when you create your monitoring schedule\. For details about how to create time offsets, see [](model-monitor-model-quality-schedule.md#model-monitor-model-quality-schedule-offsets)\. 

If you have submitted a baselining job, the monitor automatically picks up analysis configuration from the baselining job\. If you skip the baselining step or the capture dataset has a different nature from the training dataset, you must provide the analysis configuration\.

## Bias drift monitoring for models deployed to real\-time endpoint<a name="model-monitor-bias-quality-rt"></a>

To schedule a bias drift monitor for a real\-time endpoint, pass your `EndpointInput` instance to the `endpoint_input` argument of your `ModelBiasModelMonitor` instance, as shown in the following code sample:

```
from sagemaker.model_monitor import CronExpressionGenerator
            
model_bias_monitor = ModelBiasModelMonitor(
    role=sagemaker.get_execution_role(),
    ...
)

model_bias_analysis_config = None
if not model_bias_monitor.latest_baselining_job:
    model_bias_analysis_config = BiasAnalysisConfig(
        model_bias_config,
        headers=all_headers,
        label=label_header,
    )

model_bias_monitor.create_monitoring_schedule(
    monitor_schedule_name=schedule_name,
    post_analytics_processor_script=s3_code_postprocessor_uri,
    output_s3_uri=s3_report_path,
    statistics=model_bias_monitor.baseline_statistics(),
    constraints=model_bias_monitor.suggested_constraints(),
    schedule_cron_expression=CronExpressionGenerator.hourly(),
    enable_cloudwatch_metrics=True,
    analysis_config=model_bias_analysis_config,
    endpoint_input=EndpointInput(
        endpoint_name=endpoint_name,
        destination="/opt/ml/processing/input/endpoint",
        start_time_offset="-PT1H",
        end_time_offset="-PT0H",
        probability_threshold_attribute=0.8,
    ),
)
```

## Bias drift monitoring for batch transform jobs<a name="model-monitor-bias-quality-bt"></a>

To schedule a bias drift monitor for a batch transform job, pass your `BatchTransformInput` instance to the `batch_transform_input` argument of your `ModelBiasModelMonitor` instance, as shown in the following code sample:

```
from sagemaker.model_monitor import CronExpressionGenerator
                
model_bias_monitor = ModelBiasModelMonitor(
    role=sagemaker.get_execution_role(),
    ...
)

model_bias_analysis_config = None
if not model_bias_monitor.latest_baselining_job:
    model_bias_analysis_config = BiasAnalysisConfig(
        model_bias_config,
        headers=all_headers,
        label=label_header,
    )
    
schedule = model_bias_monitor.create_monitoring_schedule(
   monitor_schedule_name=schedule_name,
   post_analytics_processor_script=s3_code_postprocessor_uri,
   output_s3_uri=s3_report_path,
   statistics=model_bias_monitor.baseline_statistics(),
   constraints=model_bias_monitor.suggested_constraints(),
   schedule_cron_expression=CronExpressionGenerator.hourly(),
   enable_cloudwatch_metrics=True,
   analysis_config=model_bias_analysis_config,
   batch_transform_input=BatchTransformInput(
        destination="opt/ml/processing/input",
        data_captured_destination_s3_uri=s3_capture_path,
        start_time_offset="-PT1H",
        end_time_offset="-PT0H",
        probability_threshold_attribute=0.8
   ),
)
```