# CloudWatch Metrics for Bring Your Own Containers<a name="model-monitor-byoc-cloudwatch"></a>

If the `publish_cloudwatch_metrics` value is `Enabled` in the `Environment` map in the `/opt/ml/processing/processingjobconfig.json` file, the container code emits Amazon CloudWatch metrics in this location: `/opt/ml/output/metrics/cloudwatch`\. 

The schema for this file is closely based on the CloudWatch `PutMetrics` API\. The namespace is not specified here\. It defaults to the following:
+ `For real-time endpoints: /aws/sagemaker/Endpoint/data-metrics`
+ `For batch transform jobs: /aws/sagemaker/ModelMonitoring/data-metrics`

However, you can specify dimensions\. We recommend you add the following dimensions at minimum:
+ `Endpoint` and `MonitoringSchedule` for real\-time endpoints
+ `MonitoringSchedule` for batch transform jobs

The following JSON snippets show how to set your dimensions\.

For a real\-time endpoint, see the following JSON snippet which includes the `Endpoint` and `MonitoringSchedule` dimensions:

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

For a batch transform job, see the following JSON snippet which includes the `MonitoringSchedule` dimension:

```
{ 
    "MetricName": "", # Required
    "Timestamp": "2019-11-26T03:00:00Z", # Required
    "Dimensions" : [{"Name":"MonitoringSchedule","Value":"schedule_0"}]
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