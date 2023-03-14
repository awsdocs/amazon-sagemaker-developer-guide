# Inspect Reports for Feature Attribute Drift in Production Models<a name="clarify-feature-attribute-drift-report"></a>

After the schedule that you set up is started by default, you need to wait for the its first execution to start, and then stop the schedule to avoid incurring charges\.

To inspect the reports, use the following code:

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

 If there are any violations compared to the baseline, they are listed here:

```
if last_model_explainability_monitor_execution:
    model_explainability_violations = last_model_explainability_monitor_execution.constraint_violations()
    if model_explainability_violations:
        print(model_explainability_violations.body_dict)
```

If your model is deployed to a real\-time endpoint, you can see visualizations in SageMaker Studio of the analysis results and CloudWatch metrics by choosing the **Endpoints** tab, and then double\-clicking the endpoint\.