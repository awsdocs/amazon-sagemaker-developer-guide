# Profile Training Jobs Using Amazon SageMaker Debugger<a name="debugger-profile-training-jobs"></a>

To profile compute resource utilization and framework operations of your training job, use profiling tools offered by Amazon SageMaker Debugger\. 

For any training job you run in SageMaker using the SageMaker Python SDK, Debugger starts profiling basic resource utilization metrics, such as CPU utilization, GPU utilization, GPU memory utilization, network, and I/O wait time\. It collects these resource utilization metrics every 500 milliseconds\. To see the graphs of the resource utilization metrics of your training job, simply use the [SageMaker Debugger UI in SageMaker Studio Experiments](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-on-studio.html)\.

Deep learning operations and steps might operate in intervals of milliseconds\. Compared to Amazon CloudWatch metrics, which collect metrics at intervals of 1 second, Debugger provides finer granularity into the resource utilization metrics down to 100\-millisecond \(0\.1 second\) intervals so you can dive deep into the metrics at the level of an operation or a step\. 

If you want to change the metric collection time interval, you need to add parameters for profiling to your training job launcher\. If you're using SageMaker Python SDK, you need to pass the `profiler_config` parameter when you create an estimator\. To learn how to adjust the resource utilization metric collection interval, see [Construct a SageMaker Estimator with SageMaker Debugger](debugger-configuration-for-profiling.md#debugger-configuration-structure-profiler) and then [Configure Debugger for Monitoring Resource Utilization](debugger-configure-system-monitoring.md)\.

Additionally, you can add profiling analysis tools called *built\-in profiling rules* provided by SageMaker Debugger\. The built\-in profiling rules run analysis against the resource utilization metrics and detect computational performance issues\. For more information, see [Configure Built\-in Profiling Rules Managed by Amazon SageMaker Debugger](use-debugger-built-in-profiler-rules.md)\. You can receive rule analysis results through the [SageMaker Debugger UI in SageMaker Studio Experiments](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-on-studio.html) or the [SageMaker Debugger Profiling Report](http://dev-dsk-cmiyoung-2a-c844f850.us-west-2.amazon.com/sagemaker/AWSIronmanApiDoc/alpha/cmiyoung-tornasole/latest/dg/debugger-profiling-report.html)\. You can also create custom profiling rules using the SageMaker Python SDK\. 

Use the following topics to learn more about profiling functionalities provided by SageMaker Debugger\.

**Topics**
+ [Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration-for-profiling.md)
+ [Configure Built\-in Profiling Rules Managed by Amazon SageMaker Debugger](use-debugger-built-in-profiler-rules.md)
+ [Amazon SageMaker Debugger UI in Amazon SageMaker Studio Experiments](debugger-on-studio.md)
+ [SageMaker Debugger Interactive Report](debugger-report.md)
+ [Analyze Data Using the SMDebug Client Library](debugger-analyze-data.md)