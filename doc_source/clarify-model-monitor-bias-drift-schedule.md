# Schedule feature attribute drift monitoring jobs<a name="clarify-model-monitor-bias-drift-schedule"></a>

Now that you have a baseline, you are ready to call the `create_monitoring_schedule()` method to schedule a hourly monitor, to analyze the data with monitoring schedule\. If a baselining job has been submitted, then the monitor automatically pick up analysis configuration from the baselining job\. But if the baselining step is skipped, or the capture dataset has different nature than the training dataset, then analysis configuration has to be provided\.

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