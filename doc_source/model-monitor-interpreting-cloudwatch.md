# CloudWatch Metrics<a name="model-monitor-interpreting-cloudwatch"></a>

**Built\-in Amazon SageMaker Model Monitor container for CloudWatch metrics**: When the `emit_metrics` option is `Enabled` in the baseline constraints file, SageMaker emits these metrics for each feature/column observed in the dataset in the `/aws/sagemaker/Endpoints/data-metric` namespace with `EndpointName` and `ScheduleName` dimensions\.

For numerical fields:
+ Metric : Max → query for `MetricName: feature_data_{feature_name}, Stat: Max`
+ Metric : Min → query for `MetricName: feature_data_{feature_name}, Stat: Min`
+ Metric : Sum → query for `MetricName: feature_data_{feature_name}, Stat: Sum`
+ Metric : SampleCount → query for `MetricName: feature_data_{feature_name}, Stat: SampleCount`
+ Metric : Average → query for `MetricName: feature_data_{feature_name}, Stat: Average`

For both numerical and string fields:
+ Metric: Completeness → query for `MetricName: feature_non_null_{feature_name}, Stat: Sum`
+ Metric: Baseline Drift → query for `MetricName: feature_baseline_drift_{feature_name}, Stat: Sum`