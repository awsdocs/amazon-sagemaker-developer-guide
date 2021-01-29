# Debugger on Studio<a name="debugger-on-studio"></a>

Use the Debugger dashboards on Studio to analyze your model performance on Amazon EC2 instances\. For any SageMaker training jobs running on Amazon EC2 instance, by default, Debugger monitors system metrics \(CPU, GPU, CPU and GPU memory, Network, and data I/O\) every 500 milliseconds and basic output tensors \(loss and accuracy\) every 500 iterations\. You can also further customize Debugger configuration parameter values and adjust the saving intervals through the Studio UI or using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. Through the Studio Debugger dashboards, gain insights into your training jobs and improve your model training performance and accuracy\.

**Important**  
If using existing SageMaker Studio apps, you need to restart them to use the new features\. To restart and update your Studio environment, follow the instructions at [Update Amazon SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-update.html)\. 

![\[An example of a SageMaker Studio Debugger dashboard\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-sample.png)

**To open the Debugger dashboards**

1. Choose SageMaker **Components and registries**, open the drop down menu, and choose **Experiments and trials**\.  
![\[SageMaker Studio trial drop down menu\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-dropdown.png)

1. Look up your training job name to open the Debugger dashboards\. If you have not specified a SageMaker Experiments object, your training job trial is collected under the *Unassigned trial components* list\.

1. Open the drop down menu of the training job\. There are two menu items to browse the Debugger dashboards: **Open Debugger for insights** and **Open in trial details**\.  
![\[SageMaker Studio trial drop down menu\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-dropdown-2.png)
+ **Open Debugger for insights**

  Opens a **Debug \[your\-training\-job\-name\]** tab\. In this tab, Debugger provides an overview of your model performance on EC2 instances and identifies system bottleneck problems\. While **monitoring** your model performance, you can also enable **profiling** to capture framework metrics, which consist of data from neural network operations executed during forward and backward pass and data loading\. Debugger correlates the system resource utilization metrics with the framework metrics, and helps identify resource\-intensive operators that might be the root cause of the system bottlenecks\. You can also download an aggregated Debugger profiling report\. For more information, see [Debugger for Insights on Studio](debugger-on-studio-insights.md)\.
+ **Open in trial details**

  Opens a **Describe Trial Component** tab\. You can check **Charts** and **Metrics** of the model output data captured by Debugger and logged by a SageMaker Experiments Tracker\. Under the **Debugger** tab, you can check the status of Debugger rules in real time\. For more information, see [Debugger in Studio Experiments](debugger-on-studio-experiments.md)\.

  See the following topics to learn more about **Debugger Insights on Studio** and **Debugger in Studio Experiment List Trial**\.

**Topics**
+ [Debugger for Insights on Studio](debugger-on-studio-insights.md)
+ [Debugger in Studio Experiments](debugger-on-studio-experiments.md)