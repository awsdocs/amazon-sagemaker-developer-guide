# The `cron` Expression for Monitoring Schedule<a name="model-monitor-schedule-expression"></a>

To provide details for the monitoring schedule, use [ `ScheduleConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ScheduleConfig.html), which is a `cron` expression that describes details about the monitoring schedule\.

Amazon SageMaker Model Monitor supports the following `cron` expressions:
+ To set the job to start every hour:

  `Hourly: cron(0 * ? * * *)`
+ To run the job daily:

  `cron(0 [00-23] ? * * *)`

For example, the following are valid `cron` expressions:
+ Daily at 12 PM UTC: `cron(0 12 ? * * *)`
+ Daily at 12 AM UTC: `cron(0 0 ? * * *)`

To support running every 6, 12 hours, Model Monitoring supports the following expression:

`cron(0 [00-23]/[01-24] ? * * *)`

For example, the following are valid `cron` expressions:
+ Every 12 hours, starting at 5 PM UTC: `cron(0 17/12 ? * * *)`
+ Every two hours, starting at 12 AM UTC: `cron(0 0/2 ? * * *)`

**Note**  
Although the `cron` expression is set to start at 5 PM UTC, note that there could be a delay of 0\-20 minutes from the actual requested time to run the execution\.
If you want to run on a daily schedule, don't provide this parameter\. SageMaker picks a time to run every day
Currently, SageMaker only supports hourly integer rates between 1 hour and 24 hours\.