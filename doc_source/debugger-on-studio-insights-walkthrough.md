# Amazon SageMaker Debugger Insights Dashboard<a name="debugger-on-studio-insights-walkthrough"></a>

When you initiate a SageMaker training job, SageMaker Debugger starts monitoring the resource utilization of the Amazon EC2 instances by default\. You can track the system utilization rates, statistics overview, and built\-in rule analysis through the insights dashboard\. This guide walks you through the content of the Debugger insights dashboard under the following tabs: **System Metrics** and **Rules**\. 

**Note**  
The Studio Debugger insights dashboard runs a Studio application on an `ml.m5.4xlarge` instance to process and render the visualizations\. Each Debugger insights tab runs one Studio kernel session\. Multiple kernel sessions for multiple Debugger insights tabs run on the single instance\. When you close a Debugger insights tab, the corresponding kernel session is also closed\. The Studio application remains active and accrues charges for the `ml.m5.4xlarge` instance usage\. For information about pricing, see the [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) page\.

**Important**  
When you are done using the Debugger insights dashboard, shut down the `ml.m5.4xlarge` instance to avoid accruing charges\. For instructions on how to shut down the instance, see [Shut Down the Amazon SageMaker Debugger Insights Instance](debugger-on-studio-insights-close.md)\.

**Important**  
In the reports, plots and recommendations are provided for informational purposes and are not definitive\. You are responsible for making your own independent assessment of the information\.

**Topics**
+ [Debugger Insights – System Metrics](debugger-on-studio-insights-sys-metrics.md)
+ [Debugger Insights – Rules](debugger-on-studio-insights-rules.md)