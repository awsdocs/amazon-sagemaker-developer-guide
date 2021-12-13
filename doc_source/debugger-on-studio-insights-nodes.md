# Debugger Insights – Nodes<a name="debugger-on-studio-insights-nodes"></a>

On the **Nodes** tab, Debugger provides detailed graphs that track each compute node on which your training jobs are running\.

**CPU and Network utilization**

The first two graphs show CPU utilization and network utilization over time\. By default, the graphs show the mean values: the average of CPU and network utilization over the total number of CPU cores\. You can select one or more CPU cores by selecting the labels to graph them on single chart and compare utilization across cores\. The timeline graphs are interactive, and the two graphs are synced up\. You can drag and zoom in and out to get a closer look at specific time windows\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-cpu.gif)

**GPU and GPU memory utilization**

The following graphs show GPU utilization and GPU memory utilization over time\. By default, the graphs show the mean utilization rate over time\. You can select the GPU core labels to see the utilization rate of each core\. Taking the mean of utilization rate over the total number of GPU cores shows the mean utilization of the entire hardware system resource\. By looking at the mean utilization rate, you can check the overall system resource usage of an Amazon EC2 instance\. The following figure shows an example training job on an ml\.p3\.16xlarge instance with 8 GPU cores\. You can monitor if the training job is well distributed, fully utilizing all GPUs\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-gpu.gif)

**Overall system utilization over time**

The following heatmap shows the entire system utilization over time projected onto the two\-dimensional plot\. Every CPU and GPU core is listed in the vertical axis, and the utilization is recorded over time with colors\. See the labeled colorbar on the right side of the plot to find out which color level corresponds to which utilization rate\. For example, in the following heatmap, after the initialization phase has ended, around Sun 23:18, you can find that the training job fully utilizes an ml\.p3\.16xlarge instance: the GPU cores are fully utilized and the CPUs are moderately used for processing Python operations\. There were several CPU bottleneck problems scattered across the CPUs at different times\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-heatmap.png)

**System resource utilization over time and framework event phase**

The **System metrics over time** graph shows the overall CPU, GPU, and data I/O utilization\. The **Framework metrics over time** graph shows the framework metrics, which are the framework event phases that you can correlate with the **System metrics over time** graph\.

You can select a time interval of interest in the system resource usage timeline, and the framework event phase spots the interval to show what events happened during the selected time interval\. In each event phase block, you can find which time interval was actually spent for the training loop and break the training loop into backward pass and forward pass events\. Overall, you can see that actual training time intervals are occupying only a small percentage over the entire training time\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-framework-metrics-graphs.gif)

**Time spent in training**

The following graph shows framework metrics from the last 30 steps of the last training loop, with cumulative time spent by different events in each step\. 

![\[An animated screenshot of a time spent in training phase plot\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-time-on-training.gif)