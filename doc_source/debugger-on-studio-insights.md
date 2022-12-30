# Open the Amazon SageMaker Debugger Insights Dashboard<a name="debugger-on-studio-insights"></a>

Open the Debugger insights dashboard in Studio to see profiling progress, results of resource utilization, and system bottlenecks of your training job running on Amazon EC2 instances\.

**Note**  
The Studio Debugger insights dashboard runs a Studio app on an `ml.m5.4xlarge` instance to process and render the visualizations\. Each Debugger insights tab runs one Studio kernel session\. Multiple kernel sessions for multiple Debugger insights tabs run on the single instance\. When you close a Debugger insights tab, the corresponding kernel session is also closed\. The Studio app remains active and accrues charges for the `ml.m5.4xlarge` instance usage\. For information about pricing, see the [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) page\.

**Important**  
When you are done using the Debugger insights dashboard, you must shut down the `ml.m5.4xlarge` instance to avoid accruing charges\. For instructions on how to shut down the instance, see [Shut Down the Amazon SageMaker Debugger Insights Instance](debugger-on-studio-insights-close.md)\.

**To open the Debugger insights dashboard**

1. In the **Home** page of Studio, choose **Experiments** in the left navigation pane\.

1. Search your training job in the **Experiments** page\. If your training job set up with an experiment run, the job should appear in the **Experiments** tab; if you didn't set up an experiment run, the job should appear in the **Unassigned runs** tab\.

1. Choose \(click\) the link of the training job name to open the job details\.

1. In the left **OVERVIEW** panel, choose **Debugs**\. In the **Debugs** page, you can browse the associated Debugger rules to your training job and their monitoring status\.

1. Choose the link of the training job name to open the Debugger insights dashboard\. This opens a **Debug \[your\-training\-job\-name\]** window\. In this window, Debugger provides an overview of the computational performance of your training job on Amazon EC2 instances and identifies system bottleneck problems\. While **monitoring** the system resource utilization, you can also enable **profiling** to capture framework metrics that consist of data from neural network operations executed during the forward and backward pass and data loading\. For more information about how to enable **profiling** using the Debugger insights dashboard controller, see [Enable and Configure Debugger Profiling for Detailed Insights](debugger-on-studio-insights-controllers.md#debugger-on-studio-insights-update-config)\. 

Debugger correlates the system resource utilization metrics with the framework metrics and helps identify resource\-intensive operators that might be the root cause of the system bottlenecks\. You can also download an aggregated Debugger profiling report\. For more information, see [Amazon SageMaker Debugger Insights Dashboard Controller](debugger-on-studio-insights-controllers.md)\.