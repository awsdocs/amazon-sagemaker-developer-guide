# CloudWatch Metrics for Feature Drift Analysis<a name="clarify-feature-attribute-drift-cw"></a>

This guide shows CloudWatch metrics and their properties that you can use for feature attribute drift analysis in SageMaker Clarify\. Feature attribute drift monitoring jobs compute and publish two types of metrics:
+ The global SHAP value of each feature\.
**Note**  
The name of this metric appends the feature name provided by the job analysis configuration to `feature_`\. For example, `feature_X` is the global SHAP value for feature `X`\.
+ The `ExpectedValue` of the metric\.

These metrics are published to the following CloudWatch namespace:
+ For real\-time endpoints: `aws/sagemaker/Endpoints/explainability-metrics`
+ For batch transform jobs: `aws/sagemaker/ModelMonitoring/explainability-metrics`

Each metric has the following properties:
+ `Endpoint`: The name of the monitored endpoint, if applicable\.
+ `MonitoringSchedule`: The name of the schedule for the monitoring job\. 
+ `ExplainabilityMethod`: The method used to compute Shapley values\. Choose `KernelShap`\.
+ `Label`: The name provided by job analysis configuration `label_headers`, or a placeholder like `label0`\.
+ `ValueType`: The type of the value returned by the metric\. Choose either `GlobalShapValues` or `ExpectedValue`\.

To stop the monitoring jobs from publishing metrics, set `publish_cloudwatch_metrics` to `Disabled` in the `Environment` map of [model explainability job](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelExplainabilityJobDefinition.html) definition\.