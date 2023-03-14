# Schedule Feature Attribute Drift Monitoring Jobs<a name="clarify-model-monitor-feature-attribute-drift-schedule"></a>

After you create your SHAP baseline, you can call the `create_monitoring_schedule()` method of your `ModelExplainabilityMonitor` class instance to schedule an hourly model explainability monitor\. The following sections show you how to create a model explainability monitor for a model deployed to a real\-time endpoint as well as for a batch transform job\.

**Important**  
You can specify either a batch transform input or an endpoint input, but not both, when you create your monitoring schedule\.

If a baselining job has been submitted, the monitor automatically picks up analysis configuration from the baselining job\. However, if you skip the baselining step or the capture dataset has a different nature from the training dataset, you have to provide the analysis configuration\. `ModelConfig` is required by `ExplainabilityAnalysisConfig` for the same reason that it's required for the baselining job\. Note that only features are required for computing feature attribution, so you should exclude Ground Truth labeling\.

## Feature attribution drift monitoring for models deployed to real\-time endpoint<a name="model-monitor-explain-quality-rt"></a>

To schedule a model explainability monitor for a real\-time endpoint, pass your `EndpointInput` instance to the `endpoint_input` argument of your `ModelExplainabilityMonitor` instance, as shown in the following code sample:

```
from sagemaker.model_monitor import CronExpressionGenerator

model_exp_model_monitor = ModelExplainabilityMonitor(
   role=sagemaker.get_execution_role(),
   ... 
)

schedule = model_exp_model_monitor.create_monitoring_schedule(
   monitor_schedule_name=schedule_name,
   post_analytics_processor_script=s3_code_postprocessor_uri,
   output_s3_uri=s3_report_path,
   statistics=model_exp_model_monitor.baseline_statistics(),
   constraints=model_exp_model_monitor.suggested_constraints(),
   schedule_cron_expression=CronExpressionGenerator.hourly(),
   enable_cloudwatch_metrics=True,
   endpoint_input=EndpointInput(
        endpoint_name=endpoint_name,
        destination="/opt/ml/processing/input/endpoint",
    )
)
```

## Feature attribution drift monitoring for batch transform jobs<a name="model-monitor-explain-quality-bt"></a>

To schedule a model explainability monitor for a batch transform job, pass your `BatchTransformInput` instance to the `batch_transform_input` argument of your `ModelExplainabilityMonitor` instance, as shown in the following code sample:

```
from sagemaker.model_monitor import CronExpressionGenerator

model_exp_model_monitor = ModelExplainabilityMonitor(
   role=sagemaker.get_execution_role(),
   ... 
)

schedule = model_exp_model_monitor.create_monitoring_schedule(
   monitor_schedule_name=schedule_name,
   post_analytics_processor_script=s3_code_postprocessor_uri,
   output_s3_uri=s3_report_path,
   statistics=model_exp_model_monitor.baseline_statistics(),
   constraints=model_exp_model_monitor.suggested_constraints(),
   schedule_cron_expression=CronExpressionGenerator.hourly(),
   enable_cloudwatch_metrics=True,
   batch_transform_input=BatchTransformInput(
        destination="opt/ml/processing/data",
        model_name="batch-fraud-detection-model",
        input_manifests_s3_uri="s3://my-bucket/batch-fraud-detection/on-schedule-monitoring/in/",
        excludeFeatures="0",
   )
)
```