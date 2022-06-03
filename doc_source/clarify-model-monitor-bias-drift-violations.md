# Bias Drift Violations<a name="clarify-model-monitor-bias-drift-violations"></a>

Bias drift jobs evaluate the baseline constraints provided by the [baseline configuration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelBiasJobDefinition.html#sagemaker-CreateModelBiasJobDefinition-request-ModelBiasBaselineConfig) against the analysis results of current `MonitoringExecution`\. If violations are detected, the job lists them to the *constraint\_violations\.json* file in the execution output location, and marks the execution status as [Interpret results](model-monitor-interpreting-results.md)\.

Here is the schema of the bias drift violations file\.
+ `facet` – The name of the facet, provided by the monitoring job analysis configuration facet `name_or_index`\. 
+ `facet_value` – The value of the facet, provided by the monitoring job analysis configuration facet `value_or_threshold`\.
+ `metric_name` – The short name of the bias metric\. For example, "CI" for class imbalance\. See [Measure Pretraining Bias](clarify-measure-data-bias.md) for the short names of each of the pretraining bias metrics and [Measure Posttraining Data and Model Bias](clarify-measure-post-training-bias.md) for the short names of each of the posttraining bias metrics\.
+ `constraint_check_type` – The type of violation monitored\. Currently only `bias_drift_check` is supported\.
+ `description` – A descriptive message to explain the violation\.

```
{
    "version": "1.0",
    "violations": [{
        "facet": "string",
        "facet_value": "string",
        "metric_name": "string",
        "constraint_check_type": "string",
        "description": "string"
    }]
}
```

If the value of a bias metric in the baseline constraints file is different from its peer in the job analysis results file \(*analysis\.json*\), the monitoring jobs log a violation\. The following output provides an example of several logged violations\.

```
{
    "version": "1.0",
    "violations": [{
        "facet": "Age",
        "facet_value": "40",
        "metric_name": "CI",
        "constraint_check_type": "bias_drift_check",
        "description": "Value 0.0751544567666083 does not meet the constraint requirement"
    }, {
        "facet": "Age",
        "facet_value": "40",
        "metric_name": "DPPL",
        "constraint_check_type": "bias_drift_check",
        "description": "Value -0.0791244970125596 does not meet the constraint requirement"
    }]
}
```