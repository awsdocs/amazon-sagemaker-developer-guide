# Schedule Monitoring Jobs<a name="model-monitor-scheduling"></a>

Amazon SageMaker Model Monitor provides you the ability to continuously monitor the data collected from the endpoints on a schedule\. You can create a monitoring schedule with the [ `CreateMonitoringSchedule`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateMonitoringSchedule.html) API with a predefined periodic interval\. For example, every *x* hours \(x can range from 1 to 23\)\.

With a Monitoring Schedule, SageMaker can kick off processing jobs at a specified frequency to analyze the data collected during a given period\. SageMaker provides a pre\-built container for performing analysis on tabular datasets\. In the processing job, SageMaker compares the dataset for the current analysis with the baseline statistics, constraints provided and generate a violations report\. In addition, CloudWatch metrics are emitted for each feature under analysis\. Alternatively, you could choose to bring your own container as outlined in the [Bring Your Own Containers](model-monitor-byoc-containers.md) topic\. 

You can create a model monitoring schedule for the endpoint created earlier\. Use the baseline resources \(constraints and statistics\) to compare against the real\-time traffic\. For this example, upload the training dataset that was used to train the pretrained model included in this example\. If you already have it in Amazon S3, you can point to it directly\.

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

Create a model monitoring schedule for the endpoint using the baseline constraints and statistics to compare against real\-time traffic\.

```
from sagemaker.model_monitor import CronExpressionGenerator
from time import gmtime, strftime

mon_schedule_name = 'DEMO-xgb-churn-pred-model-monitor-schedule-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
my_default_monitor.create_monitoring_schedule(
    schedule_name=mon_schedule_name,
    endpoint_input=predictor.endpoint,
    post_analytics_processor_script=s3_code_postprocessor_uri,
    output_s3_uri=s3_report_path,
    statistics=my_default_monitor.baseline_statistics(),
    constraints=my_default_monitor.suggested_constraints(),
    schedule_cron_expression=CronExpressionGenerator.hourly(),
    enable_cloudwatch_metrics=True,
)
```

Describe and inspect the schedule: After you describe it, observe that the `MonitoringScheduleStatus` in [ `MonitoringScheduleSummary `](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_MonitoringScheduleSummary.html) returned by the [ `ListMonitoringSchedules`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListMonitoringSchedules.html) API changes to `Scheduled`\.

```
desc_schedule_result = my_default_monitor.describe_schedule()
print('Schedule status: {}'.format(desc_schedule_result['MonitoringScheduleStatus']))
```