# Configure Debugger Monitoring Hardware System Resource Utilization<a name="debugger-configure-system-monitoring"></a>

To adjust Debugger system monitoring time intervals, use the `ProfilerConfig` API operation to create a parameter object while constructing a SageMaker framework or generic estimator depending on your preference\.

**Note**  
By default, for all SageMaker training jobs, Debugger collects resource utilization metrics from Amazon EC2 instances every 500 milliseconds for system monitoring, without any Debugger\-specific parameters specified in SageMaker estimators\.   
Debugger saves the system metrics in a default S3 bucket\. The format of the default S3 bucket URI is `s3://sagemaker-<region>-<12digit_account_id>/<training-job-name>/profiler-output/`\.

The following code example shows how to set up the `profiler_config` parameter with a system monitoring time interval of 1000 milliseconds\.

```
from sagemaker.debugger import ProfilerConfig

profiler_config=ProfilerConfig(
    system_monitor_interval_millis=1000
)
```
+  `system_monitor_interval_millis` \(int\) – Specify the monitoring intervals in milliseconds to record system metrics\. Available values are 100, 200, 500, 1000 \(1 second\), 5000 \(5 seconds\), and 60000 \(1 minute\) milliseconds\. The default value is 500 milliseconds\.

To see the progress of system monitoring, see [Open the Amazon SageMaker Debugger Insights Dashboard](debugger-on-studio-insights.md)\.