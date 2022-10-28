# Schedule Model Quality Monitoring Jobs<a name="model-monitor-model-quality-schedule"></a>

After you create your baseline, you can call the `create_monitoring_schedule()` method of your `ModelQualityMonitor` class instance to schedule an hourly model quality monitor\. The following sections show you how to create a model quality monitor for a model deployed to a real\-time endpoint as well as for a batch transform job\.

Unlike data quality monitoring, you need to supply Ground Truth labels if you want to monitor model quality\. However, Ground Truth labels could be delayed\. To address this, specify offsets when you create your monitoring schedule\. 

## Model monitor offsets<a name="model-monitor-model-quality-schedule-offsets"></a>

Model quality jobs include `StartTimeOffset` and `EndTimeOffset`, which are fields of the `ModelQualityJobInput` parameter of the `create_model_quality_job_definition` method that work as follows:
+ `StartTimeOffset` \- If specified, jobs subtract this time from the start time\.
+ `EndTimeOffset` \- If specified, jobs subtract this time from the end time\.

The format of the offsets are, for example, \-PT7H, where 7H is 7 hours\. You can use \-PT\#H or \-P\#D, where H=hours, D=days, and M=minutes, and \# is the number\. In addition, the offset should be in [ISO 8601 duration format](https://en.wikipedia.org/wiki/ISO_8601#Durations)\.

For example, if your Ground Truth starts coming in after 1 day, but is not complete for a week, set `StartTimeOffset` to `-P8D` and `EndTimeOffset` to `-P1D`\. Then, if you schedule a job to run at `2020-01-09T13:00`, it analyzes data from between `2020-01-01T13:00` and `2020-01-08T13:00`\.

**Important**  
The schedule cadence should be such that one execution finishes before the next execution starts, which allows the Ground Truth merge job and monitoring job from the execution to complete\. The maximum runtime of an execution is divided between the two jobs, so for an hourly model quality monitoring job, the value of `MaxRuntimeInSeconds` specified as part of `StoppingCondition` should be no more than 1800\.

## Model quality monitoring for models deployed to real\-time endpoints<a name="model-monitor-data-quality-schedule-rt"></a>

To schedule a model quality monitor for a real\-time endpoint, pass your `EndpointInput` instance to the `endpoint_input` argument of your `ModelQualityMonitor` instance, as shown in the following code sample:

```
from sagemaker.model_monitor import CronExpressionGenerator
                    
model_quality_model_monitor = ModelQualityMonitor(
   role=sagemaker.get_execution_role(),
   ...
)

schedule = model_quality_model_monitor.create_monitoring_schedule(
   monitor_schedule_name=schedule_name,
   post_analytics_processor_script=s3_code_postprocessor_uri,
   output_s3_uri=s3_report_path,
   schedule_cron_expression=CronExpressionGenerator.hourly(),    
   statistics=model_quality_model_monitor.baseline_statistics(),
   constraints=model_quality_model_monitor.suggested_constraints(),
   schedule_cron_expression=CronExpressionGenerator.hourly(),
   enable_cloudwatch_metrics=True,
   endpoint_input=EndpointInput(
        endpoint_name=endpoint_name,
        destination="/opt/ml/processing/input/endpoint",
        start_time_offset="-PT2D",
        end_time_offset="-PT1D",
    )
)
```

## Model quality monitoring for batch transform jobs<a name="model-monitor-data-quality-schedule-tt"></a>

To schedule a model quality monitor for a batch transform job, pass your `BatchTransformInput` instance to the `batch_transform_input` argument of your `ModelQualityMonitor` instance, as shown in the following code sample:

```
from sagemaker.model_monitor import CronExpressionGenerator

model_quality_model_monitor = ModelQualityMonitor(
   role=sagemaker.get_execution_role(),
   ...
)

schedule = model_quality_model_monitor.create_monitoring_schedule(
    monitor_schedule_name=mon_schedule_name,
    batch_transform_input=BatchTransformInput(
        data_captured_destination_s3_uri=s3_capture_upload_path,
        destination="/opt/ml/processing/input",
        dataset_format=MonitoringDatasetFormat.csv(header=False),
        # the column index of the output representing the inference probablity
        probability_attribute="0",
        # the threshold to classify the inference probablity to class 0 or 1 in 
        # binary classification problem
        probability_threshold_attribute=0.5,
        # look back 6 hour for transform job outputs.
        start_time_offset="-PT6H",
        end_time_offset="-PT0H"
    ),
    ground_truth_input=gt_s3_uri,
    output_s3_uri=s3_report_path,
    problem_type="BinaryClassification",
    constraints = constraints_path,
    schedule_cron_expression=CronExpressionGenerator.hourly(),
    enable_cloudwatch_metrics=True,
)
```