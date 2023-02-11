# Bias Drift Violations<a name="clarify-model-monitor-bias-drift-violations"></a>

Bias drift jobs evaluate the baseline constraints provided by the [baseline configuration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelBiasJobDefinition.html#sagemaker-CreateModelBiasJobDefinition-request-ModelBiasBaselineConfig) against the analysis results of current `MonitoringExecution`\. If violations are detected, the job lists them to the *constraint\_violations\.json* file in the execution output location, and marks the execution status as [Interpret results](model-monitor-interpreting-results.md)\.

Here is the schema of the bias drift violations file\.
+ `facet` – The name of the facet, provided by the monitoring job analysis configuration facet `name_or_index`\. 
+ `facet_value` – The value of the facet, provided by the monitoring job analysis configuration facet `value_or_threshold`\.
+ `metric_name` – The short name of the bias metric\. For example, "CI" for class imbalance\. See [Measure Pretraining Bias](clarify-measure-data-bias.md) for the short names of each of the pre\-training bias metrics and [Measure Post\-training Data and Model Bias](clarify-measure-post-training-bias.md) for the short names of each of the post\-training bias metrics\.
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

A bias metric is used to measure the level of equality in a distribution\. A value close to zero indicates that the distribution is more balanced\. If the value of a bias metric in the job analysis results file \(analysis\.json\) is worse than its corresponding value in the baseline constraints file, a violation is logged\. As an example, if the baseline constraint for the DPPL bias metric is `0.2`, and the analysis result is `0.1`, no violation is logged because `0.1` is closer to `0` than `0.2`\. However, if the analysis result is `-0.3`, a violation is logged because it is farther from `0` than the baseline constraint of `0.2`\.

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