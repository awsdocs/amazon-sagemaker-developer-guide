# Container Contract Outputs<a name="model-monitor-byoc-contract-outputs"></a>

The container can analyze the data available in the `*dataset_source*` path and write reports to the path in `*output_path*.` The container code can write any reports that suit your needs\.

If you use the following structure and contract, certain output files are treated specially by SageMaker in the visualization and API affordances This applies only to tabular datasets\.


**Table: Output Files for Tabular Datasets**  

| File Name | Description | 
| --- | --- | 
| statistics\.json |  This file is expected to have columnar statistics for each feature in the dataset that is analyzed\. See the schema for this file in the next section\.  | 
| constraints\.json |  This file is expected to have the constraints on the features observed\. See the schema of this file below\.  | 
| constraints\_violations\.json |  This file is expected to have the list of violations found in this current set of data as compared to the baseline statistics and constraints file specified in the `baseline_constaints` and `baseline_statistics` path\.  | 

In addition, if the `publish_cloudwatch_metrics` value is `"Enabled"` container code can emit Amazon CloudWatch metrics in this location: `/opt/ml/output/metrics/cloudwatch`\. The schema for these files is described in the following sections\.

**Topics**
+ [Schema for Statistics \(statistics\.json file\)](model-monitor-byoc-statistics.md)
+ [Schema for Constraints \(constraints\.json file\)](model-monitor-byoc-constraints.md)