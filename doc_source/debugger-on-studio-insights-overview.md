# Debugger Insights – Overview<a name="debugger-on-studio-insights-overview"></a>

On the **Overview** tab, you can find a training job summary, resource utilization summary, resource intensive operations, and insights\.

## Training job summary<a name="debugger-on-studio-insights-overview-summary"></a>

The **Training job summary** section shows the overall training time spent on different phases of training: initialization, training loop, and finalization\. The pie chart shows the time usage percentage and absolute time amount spent on the different training phases\. For example, you can have a high\-level overview of how long it takes for initializing a training job, check if the initialization is taking too long due to data downloading, leaving the GPUs idle\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-training-job-summary.gif)

This section has the following features: 
+ The **Training progress over time** chart shows the timeline of the different training phases over time\. If you're using spot training, you can also find the spot interruptions in the timeline chart\.
+ The **Training job details** panel shows the exact time stamps and utilization rate percentage numbers\.
  + **Start time**: The exact time when the training job started\.
  + **End time**: The exact time when the training job finished\.
  + **Job duration**: The total training time from the **Start time** to the **End time**\.
  + **Training loop start**: The exact time when the first step of the first epoch has started\.
  + **Training loop end**: The exact time when the last step of the last epoch has finished\.
  + **Training loop duration**: The total time between the training loop start time and the training loop end time\.
  + **Initialization**: Time spent on initializing the training job, such as compiling the training script, initiating Amazon EC2 instances, and downloading training data\.
  + **Finalization**: Time spent on finalizing the training job, such as finishing the model training, updating the model artifacts, and closing the Amazon EC2 instances\.
  + **Initialization \(%\)**: The percentage of time spent on **Initialization** over the total **Job duration**\. 
  + **Training loop \(%\)**: The percentage of time spent on **Training loop** over the total **Job duration**\.
  + **Finalization \(%\)**: The percentage of time spent on **Finalization** over the total **Job duration**\.

## Resource utilization summary<a name="debugger-on-studio-insights-resource-summary"></a>

This summary table shows hardware system resource utilization statistics of all workers \(algo\-n\)\. System metrics include total CPU utilization, total GPU utilization, total CPU memory utilization, total GPU memory utilization, total I/O wait time, and total network in bytes\. The table shows the minimum and the maximum values, and p99, p90, and p50 percentiles\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-resource-util-summary.png)

## Resource intensive operations<a name="debugger-on-studio-insights-resource-operations"></a>

The **Resource intensive operations** section provides more detailed profiling results that show which training job operations were compute intensive\. In the following example, it shows that the convolutional neural network backward pass operators were the most resource intensive on the GPUs\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-resource-intensive-op.png)



## Insights<a name="debugger-on-studio-insights-pane"></a>

In the **Insights** pane, you can find training issues detected by Debugger built\-in rules\. You can expand each entry in the list to find useful insights, suggestions, a description of the rule, and criteria for initiating the rule\.

For more information about the Debugger built\-in rules, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-rules.gif)