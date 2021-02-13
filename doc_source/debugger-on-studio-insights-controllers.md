# SageMaker Debugger Insights Dashboard Controller<a name="debugger-on-studio-insights-controllers"></a>

There are different components of the Debugger Controller for monitoring and profiling\. In this guide, you learn about the Debugger controller components\.

**Note**  
Studio Debugger insights dashboard runs a Studio app on an `ml.m5.4xlarge` instance to process and render the visualizations\. Each Debugger insights tab runs one Studio kernal session\. Multiple kernal sessions for multiple Debugger insights tabs run on the single instance\. When you close a Debugger insights tab, the corresponding kernel session is also closed\. A Studio app remains active and accrue charges for the `ml.m5.4xlarge` instance usage\. For information about pricing, see the [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) page\.

**Important**  
When you are done using the Debugger insights dashboard, you must shut down the `ml.m5.4xlarge` instance to avoid accruing charges\. For instructions on how to shut down the instance, see [Shut Down the SageMaker Debugger Insights Instance](debugger-on-studio-insights-close.md)\.

## SageMaker Debugger Insights Controller UI<a name="debugger-on-studio-insights-controller"></a>

Using the Debugger controller located at the upper\-left corner of the insights dashboard, you can refresh the dashboard, configure or update Debugger settings for monitoring system metrics and profiling framework metrics, stop training job, and download Debugger profiling report\.

![\[Debugger Insights Dashboard Controllers\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-refresh.png)
+ If you want to manually refresh the **Debug \[your\-training\-job\-name\]** page, choose the refresh button \(the round arrow at the upper\-left corner\) as shown in the preceeding screenshot\. 
+ **Monitoring** is on by default for any SageMaker training job\. By monitoring your training job, Debugger only collects system metrics to detect resource utilization problems, such as CPU bottlenecks and GPU underutilization\. For a complete list of resource utilization problems that Debugger monitors, see [Debugger Built\-in Rules for Monitoring Hardware System Resource Utilization \(System Metrics\)](debugger-built-in-rules.md#built-in-rules-monitoring)\.
+ To download a comprehensive Debugger profiling report with details and analysis of a training job, choose **Download report**\. For more information about the Debugger profiling report, see [SageMaker Debugger Profiling Report](debugger-profiling-report.md)\.

## Enable and Configure Debugger Profiling for Detailed Insights<a name="debugger-on-studio-insights-update-config"></a>

When you enable **Profiling**, Debugger starts collecting framework metrics\. Framework metrics are model data collected from ML framework operations of your model, such as forward pass, backward pass, batch normalization, and data loader processes\. Debugger correlates system performance bottlenecks with the framework operations and runs the [Debugger Built\-in Rules for Profiling Framework Metrics](debugger-built-in-rules.md#built-in-rules-profiling)\.

**Note**  
After you've enabled profiling, Debugger collects every framework operation call that's executed in each step: operations for convolving down input layers during forward pass, updating weights of millions of neurons during backward pass, and data loader processes\. While profiling enables you to understand model performance at a deeper level, collecting framework metrics might impact your training time and performance\. We recommend that you enable profiling to inspect your model up to two steps at a time\. For more information about how to configure Debugger for framework profiling using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), see [Configure Debugger Framework Profiling](debugger-configure-framework-profiling.md) and [Updating Debugger System Monitoring and Framework Profiling Configuration while a Training Job is Running](debugger-update-monitoring-profiling.md)\.

If you want to enable profiling while your training job is running, use the following steps to start profiling\.

1. In SageMaker Studio, turn on **Profiling** to enable Debugger framework profiling\. This opens a Debugger monitoring and profiling configuration page\.  
![\[Turn on Profiling using the Debugger Insights Dashboard Controller UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-1.png)

1. In **Configure Debugger monitoring and profiling**, **S3 bucket URI** and **Collect monitoring data every** are already set to the default values\. 

   You can choose to change the monitoring interval using the drop down menu and selecting among the following available options: 100 milliseconds, 200 milliseconds, 500 milliseconds \(default\), 1 second, 5 seconds, and 1 minute\.   
![\[debugger-studio-insight-mockup\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-enable-profiling-2.png)

   The following fields need to be specified:
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