# CloudWatch Metrics for Bias Drift Analysis<a name="clarify-model-monitor-bias-drift-cw"></a>

This guide shows CloudWatch metrics and their properties that you can use for bias drift analysis in SageMaker Clarify\. Bias drift monitoring jobs compute both [pretraining bias metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-data-bias.html) and [posttraining bias metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-post-training-bias.html), and publish them to the following CloudWatch namespace:
+ For real\-time endpoints: `aws/sagemaker/Endpoints/bias-metrics`
+ For batch transform jobs: `aws/sagemaker/ModelMonitoring/bias-metrics` 

The CloudWatch metric name appends the metric's short name to `bias_metric`\.

For example, `bias_metric_CI` is the bias metric for class imbalance \(CI\)\.

**Note**  
`+/- infinity` is published as the floating point number `+/- 2.348543e108`, and errors including null values are not published\.

Each metric has the following properties:
+ `Endpoint`: The name of the monitored endpoint, if applicable\.
+ `MonitoringSchedule`The name of the schedule for the monitoring job\. 
+ `BiasStage`: The name of the stage of the bias drift monitoring job\. Choose either `Pre-training` or `Post-Training`\.
+ `Label`: The name of the target feature, provided by the monitoring job analysis configuration `label`\.
+ `LabelValue`: The value of the target feature, provided by the monitoring job analysis configuration `label_values_or_threshold`\.
+ `Facet`: The name of the facet, provided by the monitoring job analysis configuration facet `name_of_index`\.
+ `FacetValue`: The value of the facet, provided by the monitoring job analysis configuration facet `nvalue_or_threshold`\.

To stop the monitoring jobs from publishing metrics, set `publish_cloudwatch_metrics` to `Disabled` in the `Environment` map of [model bias job](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelBiasJobDefinition.html) definition\.