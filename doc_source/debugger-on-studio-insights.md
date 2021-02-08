# Debugger for Insights on Studio<a name="debugger-on-studio-insights"></a>

When you initiate a SageMaker training job, Debugger starts monitoring hardware system resource utilization of EC2 instances on which your training job is running\. You can track the system utilization status, statistics overview, and bottleneck issues through the Studio 

**Note**  
Studio Debugger insights dashboard runs a Studio kernel on an `ml.m5.4xlarge` instance to process and render the visualizations\. When you close the Debugger insights tab, the corresponding kernel session will also be closed\. Each Debugger insights tab runs one Studio kernal session\. For information about pricing, see the [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) page\.

## Open Debugger for Insights on Studio<a name="debugger-on-studio-insights-open"></a>

This guide walks you through every component of the Studio Debugger insights dashboard\. The following animated screenshot shows a quick overview of the Studio Debugger insights dashboard that you can see after choosing a training job trial in **Experiment List** and **Open Debugger for insights**\. On the **Debug \[your\-training\-job\-name\]** page, Debugger autogenerates model training performance summaries and insights on the **Overview** tab, and charts on the **Nodes** tab in real time\.

**Note**  
It might take a few minutes to load the Debugger insights dashboard if you are using it for the first time\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-mockup.gif)

**Tips**  
If you want to manually refresh the **Debug \[your\-training\-job\-name\]** page, choose the refresh button \(the round arrow at the upper\-left corner\) as shown in the following screenshot\. 
To download a comprehensive Debugger profiling report with more details and analysis, choose **Download report**\. For more information about the Debugger profiling report, see [SageMaker Debugger Profiling Report](debugger-profiling-report.md)\.  

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-refresh.png)

## Enable Debugger Profiling for Detailed Insights<a name="debugger-on-studio-insights-update-config"></a>

When you enable **Profiling**, Debugger starts collecting framework metrics\. Framework metrics are model data collected from ML framework operations of your model, such as forward pass, backward pass, batch normalization, and data loader processes\. Debugger correlates system performance bottlenecks with the framework operations, and helps improve the performance of your model with a fleet of Debugger built\-in rules\. 

**Note**  
After you've enabled profiling, Debugger collects every framework operation call that's executed in each step: operations for convolving down input layers during forward pass, updating weights of millions of neurons during backward pass, and data loader processes\. While profiling enables you to understand model performance at a deeper level, collecting framework metrics might impact your training time and performance\. We recommend that you enable profiling to inspect your model up to two steps at a time\. For more information about how to configure Debugger for framework profiling using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), see [Configure Debugger Framework Profiling](debugger-configure-framework-profiling.md) and [Updating Debugger System Monitoring and Framework Profiling Configuration while a Training Job is Running](debugger-update-monitoring-profiling.md)\.

If you want to enable profiling while your training job is running, use the following steps to start profiling\.

1. In SageMaker Studio, turn on **Profiling** to enable Debugger framework profiling\.  
![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-1.png)

1. In **Configure Debugger monitoring and profiling**, **S3 bucket URI** and **Collect monitoring data every** are already set to the default values\. 

   You can choose to change the monitoring interval using the drop down menu and selecting among the following available options: 100 milliseconds, 200 milliseconds, 500 milliseconds \(default\), 1 second, 5 seconds, and 1 minute\.   
![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-2.png)
   + S3 bucket URI – Specify the base S3 bucket URI\.
   + Collect monitoring data every – Select a time interval to collect system metrics\.
**Note**  
If you choose one of the lower time intervals, you increase the granularity of monitoring system metrics\. It allows you to capture spikes and anomalies with a higher time resolution\. However, as the size of system metrics to process proportionally increases, it might impact the overall training and processing time\.

1. In **Advanced settings for profiling**, configure framework metrics profiling options\. Specify **Start step** \(or **Start time**\) and **Number of steps to profile** \(or **Time duration to profile**\) to profile\. You can also leave the input fields blank\. The default values will be automatically configured to use the current step and for 1 step duration\.  
![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-3.gif)
   + **Detailed profiling** – Specify a target step or time range to profile framework operations using the native framework profilers \(TensorFlow profiler and PyTorch profiler\)\. For example, if using TensorFlow, the Debugger hooks enable the TensorFlow profiler to collect TensorFlow\-specific framework metrics\. Detailed profiling enables you to profile all framework operators at a pre\-step \(before the first step\), within steps, and between steps of a training job\.
**Note**  
The detailed profiling might significantly increase GPU memory consumption\. It is not recommended to enable the detailed profiling for more than a couple of steps\.
   + **Python profiling** – Specify a target step or time range to profile Python functions\. You can also choose between two Python profilers: cProfile and Pyinstrument\.
     + cProfile – The standard Python profiler\. cProfile collects for every Python operator called during training\. With cProfile, Debugger saves cumulative time and annotation of each function call, providing a complete detail of Python functions\. In deep learning, for example, the most frequently called functions might be the convolutional filters and backward pass operators, and cProfile profiles every single of them\. For the cProfile option, you can further select a timer option: total time, CPU time, and off\-CPU time\. While you can profile every function calls executing on processors \(both CPU and GPU\) in CPU time, you can also identify I/O or network bottlenecks with the off\-CPU time option\. The default is total time, and Debugger profiles both CPU and off\-CPU time\. With cProfile, you are able to drill down to every single functions when analyzing the profile data\.
     + Pyinstrument – Pyinstrument is a low overhead Python profiler that works based on sampling\. With the Pyinstrument option, Debugger samples profiling events every millisecond\. Because Pyinstrument measures elapsed wallclock time instead of CPU time, the Pyinstrument option can be a better choice over the cProfile option for reducing profiling noises \(filtering out irrelevant function calls that are cumulatively fast\) and capturing operators that are actually compute intensive \(cumulatively slow\) for training your model\. With Pyinstrument, you are able to see a tree of function calls and better understand the structure and root cause of the slowness\.
**Note**  
Enabling Python profiling might result in slowing down the overall training time\. In case of cProfile, Python operators that are the most frequently called are profiled at every call, so the processing time on profiling increases with respect to the number of calls\. For Pyinstrument, the cumulative profiling time increases with respect to time because of its sampling mechanism\.
   + **Dataloader profiling** – Specify a target step or time range to profile deep learning framework data loader processes\. Debugger collects every data loader event of the frameworks\.
**Note**  
The data loader profiling can lower the training performance while collecting information from data loaders\. We don't recommend that you enable the data loader profiling for more than a couple of steps\.  
Debugger is pre\-configured to annotate data loader processes only for the AWS deep learning containers\. Debugger cannot profile data loader processes on any other custom or external containers\.

1. Choose **Confirm** to finish your profiling configuration\. When the configuration is successfully updated, you should be able to see the following confirmation message\.  
![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-4.png)

## Debugger Insights – Overview<a name="debugger-on-studio-insights-overview"></a>

On the **Overview** tab, you can find a training job summary, resource utilization summary, resource intensive operations, and insights\.

### Training job summary<a name="w1228aac24c16c19c17c11b5"></a>

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

### Resource utilization summary<a name="w1228aac24c16c19c17c11b7"></a>

This summary table shows hardware system resource utilization statistics of all workers \(algo\-n\)\. System metrics include total CPU utilization, total GPU utilization, total CPU memory utilization, total GPU memory utilization, total I/O wait time, and total Network in bytes\. The table shows the minimum and the maximum values, and p99, p90, and p50 percentiles\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-resource-util-summary.png)

### Resource intensive operations<a name="w1228aac24c16c19c17c11b9"></a>

The **Resource intensive operations** section provides more detailed profiling results that show what operations of the training job were compute intensive\. In the following example, it shows that the convolutional neural network backward pass operators were the most resource intensive on the GPUs\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-resource-intensive-op.png)



### Insights<a name="w1228aac24c16c19c17c11c11"></a>

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

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-node-time-on-training.gif)