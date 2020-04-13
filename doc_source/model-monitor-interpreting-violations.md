# Schema for Violations \(constraint\_violations\.json file\)<a name="model-monitor-interpreting-violations"></a>

The violations file is generated as the output of a `MonitoringExecution`, which lists the results of evaluating the constraints \(specified in the constraints\.json file\) against the current dataset that was analyzed\. The Amazon SageMaker Model Monitor pre\-built container provides the following violation checks\.

```
{
    "violations": [{
      "feature_name" : "string",
      "constraint_check_type" :
              "data_type_check",
            | "completeness_check",
            | "baseline_drift_check",
            | "missing_column_check",
            | "extra_column_check",
            | "categorical_values_check"
      "description" : "string"
    }]
}
```


**Table: Types of Violations Monitored**  

| Violation Check Type | Description  | 
| --- | --- | 
| data\_type\_check | If the data types in the current execution are not the same as in the baseline dataset, this violation is flagged\. During the baseline step, the generated constraints suggest the inferred data type for each column\. The `monitoring_config.datatype_check_threshold` parameter can be tuned to adjust the threshold on when it is flagged as a violation\.  | 
| completeness\_check | If the completeness \(% of non\-null items\) observed in the current execution exceeds the threshold specified in completeness threshold specified per feature, this violation is flagged\. During the baseline step, the generated constraints suggest a completeness value\.   | 
| baseline\_drift\_check | If the calculated distribution distance between the current and the baseline datasets is more than the threshold specified in `monitoring_config.comparison_threshold`, this violation is flagged\.  | 
| missing\_column\_check | If the number of columns in the current dataset is less than the number in the baseline dataset, this violation is flagged\.  | 
| extra\_column\_check | If the number of columns in the current dataset is more than the number in the baseline, this violation is flagged\.  | 
| categorical\_values\_check | If there are more unknown values in the current dataset than in the baseline dataset, this violation is flagged\. This value is dictated by the threshold in `monitoring_config.domain_content_threshold`\.  | 