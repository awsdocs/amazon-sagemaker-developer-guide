# Open the Amazon SageMaker Debugger Insights Dashboard<a name="debugger-on-studio-insights"></a>

Open the Debugger insights dashboard in Studio to see profiling progress, results of resource utilization, and system bottlenecks of your training job running on Amazon EC2 instances\.

**Note**  
The Studio Debugger insights dashboard runs a Studio app on an `ml.m5.4xlarge` instance to process and render the visualizations\. Each Debugger insights tab runs one Studio kernel session\. Multiple kernel sessions for multiple Debugger insights tabs run on the single instance\. When you close a Debugger insights tab, the corresponding kernel session is also closed\. The Studio app remains active and accrues charges for the `ml.m5.4xlarge` instance usage\. For information about pricing, see the [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) page\.

**Important**  
When you are done using the Debugger insights dashboard, you must shut down the `ml.m5.4xlarge` instance to avoid accruing charges\. For instructions on how to shut down the instance, see [Shut Down the Amazon SageMaker Debugger Insights Instance](debugger-on-studio-insights-close.md)\.

**To open the Debugger insights dashboard**

![\[An animated example of how to open the Studio Debugger insights dashboard\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-open.gif)

1. Choose the **SageMaker Components and registries** icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Components_registries.png)\)\.

1. Open the dropdown list, and choose **Experiments and trials**\.

1. Look up your training job name\. If you have not assigned a SageMaker Experiments trial component to the training job, the job is collected under the ***Unassigned trial components*** list\.

1. **Right\-click** \(or an equivalent UI interaction\) to open the context menu of the training job trial component\. There are two menu items to access the Debugger features in Studio: **Open Debugger for insights** and **Open in trial details**\.

1. Choose **Open Debugger for insights**\. This opens a **Debug \[your\-training\-job\-name\]** tab\. On this tab, Debugger provides an overview of your model training performance on Amazon EC2 instances and identifies system bottleneck problems\. While **monitoring** the system resource utilization, you can also enable **profiling** to capture framework metrics that consist of data from neural network operations executed during the forward and backward pass and data loading\. For more information about how to enable **profiling** using the Debugger insights dashboard controller, see [Enable and Configure Debugger Profiling for Detailed Insights](debugger-on-studio-insights-controllers.md#debugger-on-studio-insights-update-config)\. 

Debugger correlates the system resource utilization metrics with the framework metrics and helps identify resource\-intensive operators that might be the root cause of the system bottlenecks\. You can also download an aggregated Debugger profiling report\. For more information, see [Amazon SageMaker Debugger Insights Dashboard Controller](debugger-on-studio-insights-controllers.md)\.