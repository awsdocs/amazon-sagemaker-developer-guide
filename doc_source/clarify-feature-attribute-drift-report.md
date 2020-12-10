# Inspect reports for feature attribute drift in production models<a name="clarify-feature-attribute-drift-report"></a>

Once created the schedule is started by default, here wait for the its first execution to start, then stop the schedule to avoid incurring charges

Inspect the reports

```
schedule_desc = model_explainability_monitor.describe_schedule()
execution_summary = schedule_desc.get("LastMonitoringExecutionSummary")
if execution_summary and execution_summary["MonitoringExecutionStatus"] in ["Completed", "CompletedWithViolations"]:
    last_model_explainability_monitor_execution = model_explainability_monitor.list_executions()[-1]
    last_model_explainability_monitor_execution_report_uri = last_model_explainability_monitor_execution.output.destination
    print(f'Report URI: {last_model_explainability_monitor_execution_report_uri}')
    last_model_explainability_monitor_execution_report_files = sorted(S3Downloader.list(last_model_explainability_monitor_execution_report_uri))
    print("Found Report Files:")
    print("\n ".join(last_model_explainability_monitor_execution_report_files))
else:
    last_model_explainability_monitor_execution = None
    print("====STOP==== \n No completed executions to inspect further. Please wait till an execution completes or investigate previously reported failures.")
```

 If there are any violations compared to the baseline, they will be listed here\.

```
if last_model_explainability_monitor_execution:
    model_explainability_violations = last_model_explainability_monitor_execution.constraint_violations()
    if model_explainability_violations:
        print(model_explainability_violations.body_dict)
```

 The analysis results and CloudWatch metrics are visualized in SageMaker Studio\. Select the Endpoints tab, then double click the endpoint to show the UI\.