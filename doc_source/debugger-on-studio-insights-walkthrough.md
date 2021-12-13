# Amazon SageMaker Debugger Insights Dashboard<a name="debugger-on-studio-insights-walkthrough"></a>

When you initiate a SageMaker training job, Debugger starts monitoring hardware system resource utilization of Amazon EC2 instances by default\. You can track the system utilization rate, statistics overview, and bottleneck detection status through the insights dashboard\. This guide walks you through the content of the Debugger insights dashboard under the following tabs: **Overview**, **Nodes**, and **Model insights**\. 

**Note**  
The Studio Debugger insights dashboard runs a Studio app on an `ml.m5.4xlarge` instance to process and render the visualizations\. Each Debugger insights tab runs one Studio kernel session\. Multiple kernel sessions for multiple Debugger insights tabs run on the single instance\. When you close a Debugger insights tab, the corresponding kernel session is also closed\. The Studio app remains active and accrues charges for the `ml.m5.4xlarge` instance usage\. For information about pricing, see the [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) page\.

**Important**  
When you are done using the Debugger insights dashboard, shut down the `ml.m5.4xlarge` instance to avoid accruing charges\. For instructions on how to shut down the instance, see [Shut Down the Amazon SageMaker Debugger Insights Instance](debugger-on-studio-insights-close.md)\.

**Important**  
In the reports, plots and recommendations are provided for informational purposes and are not definitive\. You are responsible for making your own independent assessment of the information\.

**Topics**
+ [Debugger Insights – Overview](debugger-on-studio-insights-overview.md)
+ [Debugger Insights – Nodes](debugger-on-studio-insights-nodes.md)
+ [Debugger Insights – Model Insights](debugger-on-studio-insights-model-insights.md)