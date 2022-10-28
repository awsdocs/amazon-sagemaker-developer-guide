# Schedule monitoring jobs<a name="model-monitor-scheduling"></a>

Amazon SageMaker Model Monitor provides you the ability to monitor the data collected from your real\-time endpoints on a continuous schedule or from your batch transform jobs on a specific schedule\. You can create a monitoring schedule with the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateMonitoringSchedule.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateMonitoringSchedule.html) API with a predefined periodic interval\. For example, every x hours \(x can range from 1 to 23\)\.

With a monitoring schedule, SageMaker can kick off processing jobs at a specified frequency to analyze the data collected during a given period\. SageMaker provides a prebuilt container for performing analysis on tabular datasets\. In the processing job, SageMaker compares the dataset for the current analysis with the baseline statistics, constraints provided and generate a violations report\. In addition, CloudWatch metrics are emitted for each feature under analysis\. Alternatively, you could choose to bring your own container as outlined in the [Bring Your Own Containers](model-monitor-byoc-containers.md) topic\. 

You can create a model monitoring schedule for your real\-time endpoint or batch transform job\. Use the baseline resources \(constraints and statistics\) to compare against the real\-time traffic or batch job inputs\. In the following example, the training dataset used to train the model was uploaded to Amazon S3\. If you already have it in Amazon S3, you can point to it directly\.

```
# copy over the training dataset to Amazon S3 (if you already have it in Amazon S3, you could reuse it)
baseline_prefix = prefix + '/baselining'
baseline_data_prefix = baseline_prefix + '/data'
baseline_results_prefix = baseline_prefix + '/results'

baseline_data_uri = 's3://{}/{}'.format(bucket,baseline_data_prefix)
baseline_results_uri = 's3://{}/{}'.format(bucket, baseline_results_prefix)
print('Baseline data uri: {}'.format(baseline_data_uri))
print('Baseline results uri: {}'.format(baseline_results_uri))
```

```
training_data_file = open("test_data/training-dataset-with-header.csv", 'rb')
s3_key = os.path.join(baseline_prefix, 'data', 'training-dataset-with-header.csv')
boto3.Session().resource('s3').Bucket(bucket).Object(s3_key).upload_fileobj(training_data_file)
```

If you are scheduling a model monitor for a real\-time endpoint, use the baseline constraints and statistics to compare against real\-time traffic\. The following code snippet shows the general format you use to schedule a model monitor for a real\-time endpoint\.

```
from sagemaker.model_monitor import CronExpressionGenerator
from time import gmtime, strftime

mon_schedule_name = 'DEMO-xgb-churn-pred-model-monitor-schedule-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
my_default_monitor.create_monitoring_schedule(
    monitor_schedule_name=mon_schedule_name,
    endpoint_input=EndpointInput(
        endpoint_name=endpoint_name,
        destination="/opt/ml/processing/input/endpoint"
    ),
    post_analytics_processor_script=s3_code_postprocessor_uri,
    output_s3_uri=s3_report_path,
    statistics=my_default_monitor.baseline_statistics(),
    constraints=my_default_monitor.suggested_constraints(),
    schedule_cron_expression=CronExpressionGenerator.hourly(),
    enable_cloudwatch_metrics=True,
)
```

The following code snippet shows the general format you use to schedule a model monitor for a batch transform job\.

```
from sagemaker.model_monitor import (
    CronExpressionGenerator,
    BatchTransformInput, 
    MonitoringDatasetFormat, 
)
from time import gmtime, strftime

mon_schedule_name = 'DEMO-xgb-churn-pred-model-monitor-schedule-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
my_default_monitor.create_monitoring_schedule(
    monitor_schedule_name=mon_schedule_name,
    batch_transform_input=BatchTransformInput(
        destination="opt/ml/processing/input",
        data_captured_destination_s3_uri=s3_capture_upload_path,
        dataset_format=MonitoringDatasetFormat.csv(header=False),
    )
    post_analytics_processor_script=s3_code_postprocessor_uri,
    output_s3_uri=s3_report_path,
    statistics=my_default_monitor.baseline_statistics(),
    constraints=my_default_monitor.suggested_constraints(),
    schedule_cron_expression=CronExpressionGenerator.hourly(),
    enable_cloudwatch_metrics=True,
)
```

Describe and inspect the schedule: After you describe it, observe that the `MonitoringScheduleStatus` in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_MonitoringScheduleSummary.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_MonitoringScheduleSummary.html) returned by the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListMonitoringSchedules.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListMonitoringSchedules.html) API changes to `Scheduled`\.

```
desc_schedule_result = my_default_monitor.describe_schedule()
print('Schedule status: {}'.format(desc_schedule_result['MonitoringScheduleStatus']))
```