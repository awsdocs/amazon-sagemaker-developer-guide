# Schema for Constraints \(constraints\.json file\)<a name="model-monitor-byoc-constraints"></a>

A constraints\.json file is used to express the constraints that a dataset must satisfy\. Amazon SageMaker Model Monitor containers can use the constraints\.json file to evaluate datasets against\. Pre\-built containers provide the ability to generate the constraints\.json file automatically for a baseline dataset\. If you bring your own container, you can provide it with similar abilities or you can create the constraints\.json file in some other way\. Here is the schema for the constraint file that the prebuilt container uses\. Bring our own containers can adopt the same format or enhance it as required\.

```
{
  "version" : 0,

    "features": [
      {
          "name": "string",
          "inferred_type": "Integral" | "Fractional" | 
                    | "String" | "Unknown",
          "completeness": number, # denotes observed non-null value percentage
          "num_constraints" : {
            "is_non_negative": boolean,
          },
          "string_constraints" : {
             "domains": [
                  "list of",
                  "observed values",
                  "for small cardinality"
              ],
           },
           "monitoringConfigOverrides" : {
          
           }#monitoringConfigOverrides
      }#feature
  ]#features
  
  # options to control monitoring for this feature with monitoring jobs
  # See the following table for notes on what each constraint is doing.
"monitoring_config": {
    "evaluate_constraints": "Enabled",
    "emit_metrics": "Enabled",
    "datatype_check_threshold": 1.0,
    "domain_content_threshold": 1.0,
    "distribution_constraints": {
        "perform_comparison": "Enabled",
        "comparison_threshold": 0.1,
        "comparion_method": "Simple"||"Robust"
    }
}}#schema
```


**Table: Monitoring Constraints**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-byoc-constraints.html)