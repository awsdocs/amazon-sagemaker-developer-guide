# Inspect reports for data bias drift<a name="clarify-model-monitor-bias-drift-report"></a>

You are not able to inspect the results of the monitoring in the generated reports\.

```
schedule_desc = model_bias_monitor.describe_schedule()
execution_summary = schedule_desc.get("LastMonitoringExecutionSummary")
if execution_summary and execution_summary["MonitoringExecutionStatus"] in ["Completed", "CompletedWithViolations"]:
    last_model_bias_monitor_execution = model_bias_monitor.list_executions()[-1]
    last_model_bias_monitor_execution_report_uri = last_model_bias_monitor_execution.output.destination
    print(f'Report URI: {last_model_bias_monitor_execution_report_uri}')
    last_model_bias_monitor_execution_report_files = sorted(S3Downloader.list(last_model_bias_monitor_execution_report_uri))
    print("Found Report Files:")
    print("\n ".join(last_model_bias_monitor_execution_report_files))
else:
    last_model_bias_monitor_execution = None
    print("====STOP==== \n No completed executions to inspect further. Please wait till an execution completes or investigate previously reported failures.")
```

 If there are violations compared to the baseline, they will be listed here\.

```
if last_model_bias_monitor_execution:
    model_bias_violations = last_model_bias_monitor_execution.constraint_violations()
    if model_bias_violations:
        print(model_bias_violations.body_dict)
```

The analysis results and CloudWatch metrics are visualized in SageMaker Studio\. Select the Endpoints tab, then double click the endpoint to observe them in the Studio UI\.