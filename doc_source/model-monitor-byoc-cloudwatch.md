# CloudWatch Metrics<a name="model-monitor-byoc-cloudwatch"></a>

If the `publish_cloudwatch_metrics` value is `Enabled` in the `Environment` map in the `/opt/ml/processing/processingjobconfig.json` file, the container code emits Amazon CloudWatch metrics in this location: `/opt/ml/output/metrics/cloudwatch`\. 

The schema for this file is closely based on the CloudWatch `PutMetrics` API\. The namespace is not specified here\. It defaults to `/aws/sagemaker/Endpoint/data-metrics`\. However, you can specify dimensions\. We recommend that you add the `Endpoint` and `MonitoringSchedule` dimensions at a minimum\.

```
{ 
    "MetricName": "", # Required
    "Timestamp": "2019-11-26T03:00:00Z", # Required
    "Dimensions" : [{"Name":"Endpoint","Value":"endpoint_0"},{"Name":"MonitoringSchedule","Value":"schedule_0"}]
    "Value": Float,
    # Either the Value or the StatisticValues field can be populated and not both.
    "StatisticValues": {
        "SampleCount": Float,
        "Sum": Float,
        "Minimum": Float,
        "Maximum": Float
    },
    "Unit": "Count", # Optional
}
```