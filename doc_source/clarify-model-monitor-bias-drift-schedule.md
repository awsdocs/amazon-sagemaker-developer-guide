# Schedule Bias Drift Monitoring Jobs<a name="clarify-model-monitor-bias-drift-schedule"></a>

Now that you have a baseline, you can call the `create_monitoring_schedule()` method to schedule an hourly monitor to analyze the data with a monitoring schedule\. If you have submitted a baselining job, the monitor automatically picks up analysis configuration from the baselining job\. If you skip the baselining step or the capture dataset has a different nature from the training dataset, you must provide the analysis configuration\. The following code example shows how the analysis configuration is structured\.

```
model_bias_analysis_config = None
if not model_bias_monitor.latest_baselining_job:
    model_bias_analysis_config = BiasAnalysisConfig(
        model_bias_config,
        headers=all_headers,
        label=label_header,
    )
model_bias_monitor.create_monitoring_schedule(
    analysis_config=model_bias_analysis_config,
    output_s3_uri=s3_report_path,
    endpoint_input=EndpointInput(
        endpoint_name=endpoint_name,
        destination="/opt/ml/processing/input/endpoint",
        start_time_offset="-PT1H",
        end_time_offset="-PT0H",
        probability_threshold_attribute=0.8,
    ),
    ground_truth_input=ground_truth_upload_path,
    schedule_cron_expression=schedule_expression,
)
print(f"Model bias monitoring schedule: {model_bias_monitor.monitoring_schedule_name}")
```



Bias drift monitoring requires ground truth labels\. In addition, these labels are necessary to compute most SageMaker Clarify bias metrics\. To provide these, periodically label data that is captured by your endpoint and upload the labels to an Amazon S3 bucket\. Use the schema outlined in [Ingest Ground Truth Labels and Merge Them With Predictions](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-model-quality-merge.html)\. 

The execution of this bias drift monitoring job starts a merge job that joins the ground truth data and captured data together using a unique identifier\. Then merged data is saved to an Amazon S3 bucket\. Afterwards, a SageMaker Clarify bias analysis job computes bias metrics on the merged data\. The `MaxRuntimeInSeconds` parameter of the `StoppingCondition` API is divided between the merge and the analysis job\. For an hourly bias drift monitoring job, `MaxRuntimeInSeconds` should not exceed 1800 seconds\.

If the availability of ground truth labels is delayed, use the `StartTimeOffset` and `EndTimeOffset` parameters of the [EndpointInput](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StoppingCondition.html) API to adjust the start and end times of the endpoint\. For more information, see [Schedule Model Quality Monitoring Jobs](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-model-quality-schedule.html)\.