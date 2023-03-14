# Model Feature Attribution Drift Violations<a name="clarify-model-monitor-model-attribution-drift-violations"></a>

Feature attribution drift jobs evaluate the baseline constraints provided by the [baseline configuration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelExplainabilityJobDefinition.html#sagemaker-CreateModelExplainabilityJobDefinition-request-ModelExplainabilityBaselineConfig) against the analysis results of current `MonitoringExecution`\. If violations are detected, the job lists them to the *constraint\_violations\.json* file in the execution output location, and marks the execution status as [Interpret results](model-monitor-interpreting-results.md)\.

Here is the schema of the feature attribution drift violations file\.
+ `label` – The name of the label, job analysis configuration `label_headers` or a placeholder such as `"label0"`\.
+ `metric_name` – The name of the explainability analysis method\. Currently only `shap` is supported\.
+ `constraint_check_type` – The type of violation monitored\. Currently only `feature_attribution_drift_check` is supported\.
+ `description` – A descriptive message to explain the violation\.

```
{
    "version": "1.0",
    "violations": [{
        "label": "string",
        "metric_name": "string",
        "constraint_check_type": "string",
        "description": "string"
    }]
}
```

For each label in the `explanations` section, the monitoring jobs calculate the [nDCG score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.ndcg_score.html) of its global SHAP values in the baseline constraints file and in the job analysis results file \(*analysis\.json*\)\. If the score is less than 0\.9, then a violation is logged\. The combined global SHAP value is evaluated, so there are no `“feature”` fields in the violation entry\. The following output provides an example of several logged violations\.

```
{
    "version": "1.0",
    "violations": [{
        "label": "label0",
        "metric_name": "shap",
        "constraint_check_type": "feature_attribution_drift_check",
        "description": "Feature attribution drift 0.7639720923277322 exceeds threshold 0.9"
    }, {
        "label": "label1",
        "metric_name": "shap",
        "constraint_check_type": "feature_attribution_drift_check",
        "description": "Feature attribution drift 0.7323763972092327 exceeds threshold 0.9"
    }]
}
```