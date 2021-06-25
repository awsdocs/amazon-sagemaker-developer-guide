# SageMaker Debugger Insights Dashboard Walkthrough<a name="debugger-on-studio-insights-walkthrough"></a>

When you initiate a SageMaker training job, Debugger starts monitoring hardware system resource utilization of EC2 instances by default\. You can track the system utilization rate, statistics overview, and bottleneck detection status and results through the Studio\. This guide walks you through every component of the Studio Debugger insights dashboard\. 

**Note**  
Studio Debugger insights dashboard runs a Studio app on an `ml.m5.4xlarge` instance to process and render the visualizations\. Each Debugger insights tab runs one Studio kernal session\. Multiple kernal sessions for multiple Debugger insights tabs run on the single instance\. When you close a Debugger insights tab, the corresponding kernel session is also closed\. A Studio app remains active and accrue charges for the `ml.m5.4xlarge` instance usage\. For information about pricing, see the [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) page\.

**Important**  
When you are done using the Debugger insights dashboard, you must shut down the `ml.m5.4xlarge` instance to avoid accruing charges\. For instructions on how to shut down the instance, see [Shut Down the SageMaker Debugger Insights Instance](debugger-on-studio-insights-close.md)\.

**Topics**
+ [Debugger Insights – Overview](#debugger-on-studio-insights-overview)
+ [Debugger Insights – Nodes](#debugger-on-studio-insights-nodes)

## Debugger Insights – Overview<a name="debugger-on-studio-insights-overview"></a>

On the **Overview** tab, you can find a training job summary, resource utilization summary, resource intensive operations, and insights\.

### Training job summary<a name="debugger-on-studio-insights-overview-summary"></a>

The **Training job summary** section shows the overall training time spent on different phases of training: initialization, training loop, and finalization\. The pie chart shows the time usage percentage and absolute time amount spent on the different training phases\. For example, you can have a high\-level overview of how long it takes for initializing a training job, check if the initialization is taking too long due to data downloading, leaving the GPUs idle\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-training-job-summary.gif)

This section has the following features: 
+ The **Training progress over time** chart shows the timeline of the different training phase over time\. If using spot training, you can also find the spot interruptions in the timeline chart\.
+ The **Training job details** panel shows the exact time stamps and utilization rate percentage numbers\.
  + **Start time** – The exact time when the training job started\.
  + **End time** – The exact time when the training job finished\.
  + **Job duration** – The total training time from the **Start time** to the **End time**\.
  + **Training loop start** – The exact time when the first step of the first epoch has started\.
  + **Training loop end** – The exact time when the last step of the last epoch has finished\.
  + **Training loop duration** – The total time between the Training loop start time and the Training loop end time\.
  + **Initialization** – Time spent on initializing the training job, such as compiling the training script, initiating EC2 instances, and downloading training data\.
  + **Finalization** – Time spent on finalizing the training job, such as finishing the model training, updating the model artifacts, and closing the EC2 instances\.
  + **Initialization \(%\)** – The percentage of time spent on **Initialization** over the total **Job duration**\. 
  + **Training loop \(%\)** – The percentage of time spent on **Training loop** over the total **Job duration**\.
  + **Finalization \(%\)** – The percentage of time spent on **Finalization** over the total **Job duration**\.

### Resource utilization summary<a name="debugger-on-studio-insights-resource-summary"></a>

This summary table shows hardware system resource utilization statistics of all workers \(algo\-n\)\. System metrics include total CPU utilization, total GPU utilization, total CPU memory utilization, total GPU memory utilization, total I/O wait time, and total Network in bytes\. The table shows the minimum and the maximum values, and p99, p90, and p50 percentiles\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-resource-util-summary.png)

### Resource intensive operations<a name="debugger-on-studio-insights-resource-operations"></a>

The **Resource intensive operations** section provides more detailed profiling results that show what operations of the training job were compute intensive\. In the following example, it shows that the convolutional neural network backward pass operators were the most resource intensive on the GPUs\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-resource-intensive-op.png)



### Insights<a name="debugger-on-studio-insights-pane"></a>

In the **Insights** pane, you can find training issues detected by Debugger built\-in rules\. You can expand each of the list to find useful insights, suggestions, a description of the rule, and criteria of triggering the rule\.

For more information about the Debugger built\-in rules, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-rules.gif)

## Debugger Insights – Nodes<a name="debugger-on-studio-insights-nodes"></a>

On the Studio Debugger Insight **Nodes** tab, Debugger provides detailed graphs that track each compute node on which your training jobs are running\.

**CPU and Network utilization**

The first two graphs show CPU utilization and network utilization over time\. By default, the graphs show the mean values: the average of CPU and network utilization over the total number of CPU cores\. You can select one or more CPU cores, by selecting on the labels, to graph them on single chart and compare utilization across cores\. The timeline graphs are interactive, and the two graphs are synced up\. You can drag and zoom in and out to see specific time windows that you want to have a closer look\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-cpu.gif)

**GPU and GPU memory utilization**

The following graphs show GPU utilization and GPU memory utilization over time\. By default, the graphs show the mean utilization rate over time\. You can select the GPU core labels to see the utilization rate of each core\. Taking the mean of utilization rate over the total number of GPU cores shows the mean utilization of the entire hardware system resource\. By looking at the mean utilization rate, you can check the overall system resource usage of EC2 instance\. The following figure shows an example training job on an ml\.p3\.16xlarge instance with 8 GPU cores\. You can monitor if the training job is well distributed, fully utilizing all GPUs\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-gpu.gif)

**Overall system utilization over time**

The following heatmap shows the entire system utilization over time projected onto the two\-dimensional plot\. Every CPU and GPU cores are listed in the vertical axis, and the utilization is recorded over time with colors\. See the labeled colorbar on the right side of the plot to find out what color level corresponds to what utilization rate\. For example, in the following heatmap, after initialization phase has ended around Sun 23:18, you can find that the training job fully utilizes an ml\.p3\.16xlarge instance: the GPU cores are fully utilized and the CPUs are moderately used for processing Python operations\. There were several CPU bottleneck problems scattered across the CPUs at different times\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-heatmap.png)

**System resource utilization over time and framework event phase**

The **System metrics over time** graph shows the overall CPU, GPU, and data I/O utilization\. The **Framework metrics over time** graph shows the framework metrics, which are the framework event phases that you can correlate with the **System metrics over time** graph\.

You can select a time interval of interest in the system resource usage timeline, and the framework event phase spots the interval to show what events happened during the selected time interval\. In each event phase block, you can find what time interval was actually spent for training loop, and break the training loop into backward pass and forward pass events\. Overall, you can see that actual training time intervals are occupying only a small percentage over the entire training time\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-framework-metrics-graphs.gif)

**Time spent in training**

In the following graph, framework metrics from the last 30 steps of the last training loop are captured\. This graph shows cumulative time spent by different events in each step\. 

![\[An animated screenshot of a time spent in training phase plot\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-time-on-training.gif)