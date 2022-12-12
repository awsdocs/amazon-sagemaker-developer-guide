# Schedule data quality monitoring jobs<a name="model-monitor-schedule-data-monitor"></a>

After you create your baseline, you can call the `create_monitoring_schedule()` method of your `DefaultModelMonitor` class instance to schedule an hourly data quality monitor\. The following sections show you how to create a data quality monitor for a model deployed to a real\-time endpoint as well as for a batch transform job\.

## Data quality monitoring for models deployed to real\-time endpoints<a name="model-monitor-data-quality-rt"></a>

To schedule a data quality monitor for a real\-time endpoint, pass your `EndpointInput` instance to the `endpoint_input` argument of your `DefaultModelMonitor` instance, as shown in the following code sample:

```
from sagemaker.model_monitor import CronExpressionGenerator
                
data_quality_model_monitor = DefaultModelMonitor(
   role=sagemaker.get_execution_role(),
   ...
)

schedule = data_quality_model_monitor.create_monitoring_schedule(
   monitor_schedule_name=schedule_name,
   post_analytics_processor_script=s3_code_postprocessor_uri,
   output_s3_uri=s3_report_path,
   schedule_cron_expression=CronExpressionGenerator.hourly(),
   statistics=data_quality_model_monitor.baseline_statistics(),
   constraints=data_quality_model_monitor.suggested_constraints(),
   schedule_cron_expression=CronExpressionGenerator.hourly(),
   enable_cloudwatch_metrics=True,
   endpoint_input=EndpointInput(
        endpoint_name=endpoint_name,
        destination="/opt/ml/processing/input/endpoint",
   )
)
```

## Data quality monitoring for batch transform jobs<a name="model-monitor-data-quality-bt"></a>

To schedule a data quality monitor for a batch transform job, pass your `BatchTransformInput` instance to the `batch_transform_input` argument of your `DefaultModelMonitor` instance, as shown in the following code sample:

```
from sagemaker.model_monitor import CronExpressionGenerator
                
data_quality_model_monitor = DefaultModelMonitor(
   role=sagemaker.get_execution_role(),
   ...
)

schedule = data_quality_model_monitor.create_monitoring_schedule(
    monitor_schedule_name=mon_schedule_name,
    batch_transform_input=BatchTransformInput(
        data_captured_destination_s3_uri=s3_capture_upload_path,
        destination="/opt/ml/processing/input",
        dataset_format=MonitoringDatasetFormat.csv(header=False),
    ),
    output_s3_uri=s3_report_path,
    statistics= statistics_path,
    constraints = constraints_path,
    schedule_cron_expression=CronExpressionGenerator.hourly(),
    enable_cloudwatch_metrics=True,
)
```