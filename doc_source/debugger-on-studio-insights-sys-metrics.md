# Debugger Insights – System Metrics<a name="debugger-on-studio-insights-sys-metrics"></a>

On the **System Metrics** tab, you can find a statistical summary and time series graphs that show resource utilization of your training job\.

## Resource utilization summary<a name="debugger-on-studio-insights-sys-resource-summary"></a>

This summary table shows system resource utilization statistics of all nodes \(denoted as algo\-*n*\)\. System metrics include total CPU utilization, total GPU utilization, total CPU memory utilization, total GPU memory utilization, total I/O wait time, and total network in bytes\. The table shows the minimum and the maximum values, and p99, p90, and p50 percentiles\.

![\[A summary table of resource utilization\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-resource-util-summary.png)

## Resource utilization time series graphs<a name="debugger-on-studio-insights-sys-controller"></a>

Use the time series graphs to see more details of resource utilization and identify at what time interval each instance shows any undesired utilization rate, such as low GPU utilization and CPU bottlenecks that can cause a waste of the expensive instance\.

**The time series graph controller UI**

The following screenshot shows the UI controller for adjusting the time series graphs\.

![\[\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-insights-graph-controller.png)
+ **algo\-1**: Use this dropdown menu to choose the node that you want to look into\.
+ **Zoom In**: Use this button to zoom in the time series graphs and view shorter time intervals\.
+ **Zoom Out**: Use this button to zoom out the time series graphs and view wider time intervals\.
+ **Pan Left**: Move the time series graphs to an earlier time interval\.
+ **Pan Right**: Move the time series graphs to a later time interval\.
+ **Fix Timeframe**: Use this check box to fix or bring back the time series graphs to show the whole view from the first data point to the last data point\.

**CPU utilization and I/O wait time**

The first two graphs show CPU utilization and I/O wait time over time\. By default, the graphs show the average of CPU utilization rate and I/O wait time spent on the CPU cores\. You can select one or more CPU cores by selecting the labels to graph them on single chart and compare utilization across cores\. You can drag and zoom in and out to have a closer look at specific time intervals\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-insights-node-cpu.png)

**GPU utilization and GPU memory utilization**

The following graphs show GPU utilization and GPU memory utilization over time\. By default, the graphs show the mean utilization rate over time\. You can select the GPU core labels to see the utilization rate of each core\. Taking the mean of utilization rate over the total number of GPU cores shows the mean utilization of the entire hardware system resource\. By looking at the mean utilization rate, you can check the overall system resource usage of an Amazon EC2 instance\. The following figure shows an example training job on an `ml.p3.16xlarge` instance with 8 GPU cores\. You can monitor if the training job is well distributed, fully utilizing all GPUs\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-gpu.gif)

**Overall system utilization over time**

The following heatmap shows an example of the entire system utilization \(of an `ml.p3.16xlarge` instance\) over time projected onto the two\-dimensional plot\. Every CPU and GPU core is listed in the vertical axis, and the utilization is recorded over time with a color scheme, where the bright colors represent low utilization and the darker colors represent high utilization\. See the labeled color bar on the right side of the plot to find out which color level corresponds to which utilization rate\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-heatmap.png)