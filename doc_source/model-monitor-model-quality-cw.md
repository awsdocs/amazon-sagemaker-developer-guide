# Model Quality CloudWatch Metrics<a name="model-monitor-model-quality-cw"></a>

If you set the value of the `enable_cloudwatch_metrics` to `True` when you create the monitoring schedule, model quality monitoring jobs send all metrics to Amazon CloudWatch\.

Model quality metrics appear in the `aws/sagemaker/Endpoints/model-metrics` namespace\. For a list of the metrics that are emitted, see [Model Quality Metrics](model-monitor-model-quality-metrics.md)\.

You can use CloudWatch metrics to create an alarm when a specific metric doesn't meet the threshold you specify\. For instructions about how to create CloudWatch alarms, see [Create a CloudWatch Alarm Based on a Static Threshold](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ConsoleAlarms.html) in the *Amazon CloudWatch User Guide*\.