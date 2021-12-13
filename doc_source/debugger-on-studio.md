# Amazon SageMaker Debugger in Amazon SageMaker Studio<a name="debugger-on-studio"></a>

Use the Amazon SageMaker Debugger dashboards in Amazon SageMaker Studio to analyze your model performance and system bottlenecks while running training jobs on Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. Gain insights into your training jobs and improve your model training performance and accuracy with the Debugger dashboards\. By default, Debugger monitors system metrics \(CPU, GPU, CPU and GPU memory, network, and data I/O\) every 500 milliseconds and basic output tensors \(loss and accuracy\) every 500 iterations for training jobs\. You can also further customize Debugger configuration parameter values and adjust the saving intervals through the Studio UI or using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. 

**Important**  
If you're using existing Studio apps, restart them to use the new features\. For instructions on how to restart and update your Studio environment, see [Update Amazon SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-update.html)\. 

![\[An example of a Studio Debugger dashboard\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-sample.png)

**Topics**
+ [Open the Amazon SageMaker Debugger Insights Dashboard](debugger-on-studio-insights.md)
+ [Amazon SageMaker Debugger Insights Dashboard Controller](debugger-on-studio-insights-controllers.md)
+ [Amazon SageMaker Debugger Insights Dashboard](debugger-on-studio-insights-walkthrough.md)
+ [Shut Down the Amazon SageMaker Debugger Insights Instance](debugger-on-studio-insights-close.md)
+ [Amazon SageMaker Debugger in Studio Experiments](debugger-on-studio-experiments.md)