# Amazon SageMaker Debugger Insights Dashboard Controller<a name="debugger-on-studio-insights-controllers"></a>

There are different components of the Debugger controller for monitoring and profiling\. In this guide, you learn about the Debugger controller components\.

**Note**  
The Studio Debugger insights dashboard runs a Studio app on an `ml.m5.4xlarge` instance to process and render the visualizations\. Each Debugger insights tab runs one Studio kernel session\. Multiple kernel sessions for multiple Debugger insights tabs run on the single instance\. When you close a Debugger insights tab, the corresponding kernel session is also closed\. The Studio app remains active and accrues charges for the `ml.m5.4xlarge` instance usage\. For information about pricing, see the [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/) page\.

**Important**  
When you are done using the Debugger insights dashboard, shut down the `ml.m5.4xlarge` instance to avoid accruing charges\. For instructions on how to shut down the instance, see [Shut Down the Amazon SageMaker Debugger Insights Instance](debugger-on-studio-insights-close.md)\.

## SageMaker Debugger Insights Controller UI<a name="debugger-on-studio-insights-controller"></a>

Using the Debugger controller located at the upper\-left corner of the insights dashboard, you can refresh the dashboard, configure or update Debugger settings for monitoring system metrics and profiling framework metrics, stop a training job, and download a Debugger profiling report\.

![\[Debugger Insights Dashboard Controllers\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-refresh.png)
+ If you want to manually refresh the **Debug \[your\-training\-job\-name\]** page, choose the refresh button \(the round arrow at the upper\-left corner\) as shown in the preceding screenshot\. 
+ **Monitoring** is on by default for any SageMaker training job\. By monitoring your training job, Debugger only collects system metrics to detect resource utilization problems, such as CPU bottlenecks and GPU underutilization\. For a complete list of resource utilization problems that Debugger monitors, see [Debugger built\-in rules for profiling hardware system resource utilization \(system metrics\)](debugger-built-in-rules.md#built-in-rules-monitoring)\.
+ You can also download an aggregated profiling report by using the built\-in [ProfilerReport](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html#profiler-report) rule of SageMaker Debugger\. For more information, see [Configure Built\-in Profiler Rules](https://docs.aws.amazon.com/sagemaker/latest/dg/use-debugger-built-in-profiler-rules.html) and [Profiling Report Generated Using SageMaker Debugger ](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-profiling-report.html)\.

## Turn On and Configure Debugger Profiling for Detailed Insights<a name="debugger-on-studio-insights-update-config"></a>

When you turn on **Profiling**, Debugger starts collecting framework metrics\. Framework metrics are model data collected from the ML framework operations of your model, such as forward pass, backward pass, batch normalization, and data loader processes\. Debugger correlates system performance bottlenecks with the framework operations and runs the [Debugger built\-in rules for profiling framework metrics](debugger-built-in-rules.md#built-in-rules-profiling)\.

**Note**  
After you've turned on profiling, Debugger collects every framework operation call that's run in each step: operations for convolving down input layers during forward pass, updating weights of millions of neurons during backward pass, and data loader processes\. While profiling can help you understand model performance at a deeper level, collecting framework metrics might impact your training time and performance\. We recommend that you turn on profiling to inspect your model up to two steps at a time\. For more information about how to configure Debugger for framework profiling using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), see [Configure Debugger Framework Profiling](debugger-configure-framework-profiling.md) and [Updating Debugger System Monitoring and Framework Profiling Configuration while a Training Job is Running](debugger-update-monitoring-profiling.md)\.

If you want to turn on profiling while your training job is running, use the following steps to start profiling\.

1. In Studio, turn on **Profiling** to activate Debugger framework profiling\. This opens a Debugger monitoring and profiling configuration page\.  
![\[Turn on Profiling using the Debugger insights dashboard controller UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-1.png)

1. The **Configure Debugger monitoring and profiling**, **S3 bucket URI** and **Collect monitoring data every** fields are already set to the default values\. 

   You can choose to change the monitoring interval using the dropdown list\. Select from the following available options: 100 milliseconds, 200 milliseconds, 500 milliseconds \(default\), 1 second, 5 seconds, and 1 minute\.   
![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-2.png)

   Specify values for the following fields:
   + **S3 bucket URI**: Specify the base S3 bucket URI\.
   + **Collect monitoring data every**: Select a time interval to collect system metrics\.
**Note**  
If you choose one of the lower time intervals, you increase the granularity of monitoring system metrics so you can capture spikes and anomalies with a higher time resolution\. However, as the size of system metrics to process proportionally increases, it might impact the overall training and processing time\.

1. In **Advanced settings for profiling**, configure framework metrics profiling options\. Specify **Start step** \(or **Start time**\) and **Number of steps to profile** \(or **Time duration to profile**\) to profile\. You can also leave the input fields blank\. The default values are automatically configured to use the current step and for 1\-step duration\.  
![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-3.gif)
   + **Detailed profiling**: Specify a target step or time range to profile framework operations using the native framework profilers \(TensorFlow profiler and PyTorch profiler\)\. For example, if you're using TensorFlow, the TensorFlow profiler uses the Debugger hooks to collect TensorFlow\-specific framework metrics\. With detailed profiling, you can profile all framework operators at a pre\-step \(before the first step\), within steps, and between steps of a training job\.
**Note**  
The detailed profiling might significantly increase GPU memory consumption\. It is not recommended to turn on the detailed profiling for more than a couple of steps\.
   + **Python profiling**: Specify a target step or time range to profile Python functions\. You can also choose between two Python profilers: cProfile and Pyinstrument\.
     + cProfile – The standard Python profiler\. cProfile collects for every Python operator called during training\. With cProfile, Debugger saves cumulative time and annotation of each function call, providing a complete detail of Python functions\. In deep learning, for example, the most frequently called functions might be the convolutional filters and backward pass operators, and cProfile profiles every single of them\. For the cProfile option, you can further select a timer option: total time, CPU time, and off\-CPU time\. While you can profile every function calls executing on processors \(both CPU and GPU\) in CPU time, you can also identify I/O or network bottlenecks with the off\-CPU time option\. The default is total time, and Debugger profiles both CPU and off\-CPU time\. With cProfile, you can drill down to every single function when analyzing the profile data\.
     + Pyinstrument – Pyinstrument is a low overhead Python profiler that works based on sampling\. With the Pyinstrument option, Debugger samples profiling events every millisecond\. Because Pyinstrument measures elapsed wall clock time instead of CPU time, the Pyinstrument option can be a better choice over the cProfile option for reducing profiling noises \(filtering out irrelevant function calls that are cumulatively fast\) and capturing operators that are actually compute intensive \(cumulatively slow\) for training your model\. With Pyinstrument, you can see a tree of function calls and better understand the structure and root cause of the slowness\.
**Note**  
Turning on Python profiling might result in slowing down the overall training time\. In case of cProfile, Python operators that are the most frequently called are profiled at every call, so the processing time on profiling increases with respect to the number of calls\. For Pyinstrument, the cumulative profiling time increases with respect to time because of its sampling mechanism\.
   + **Dataloader profiling**: Specify a target step or time range to profile deep learning framework data loader processes\. Debugger collects every data loader event of the frameworks\.
**Note**  
The data loader profiling can lower the training performance while collecting information from data loaders\. We don't recommend that you turn on the data loader profiling for more than a couple of steps\.  
Debugger is pre\-configured to annotate data loader processes only for the AWS deep learning containers\. Debugger cannot profile data loader processes on any other custom or external containers\.

1. Choose **Confirm** to finish your profiling configuration\. When the configuration is successfully updated, you should see a **Debugger configuration updated successfully** confirmation message, as shown in following image\.  
![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-4.png)