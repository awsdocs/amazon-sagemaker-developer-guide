# Log Amazon SageMaker Events with Amazon CloudWatch<a name="logging-cloudwatch"></a>

To help you debug your processing jobs, training jobs, endpoints, transform jobs, notebook instances, and notebook instance lifecycle configurations, anything an algorithm container, a model container, or a notebook instance lifecycle configuration sends to `stdout` or `stderr` is also sent to Amazon CloudWatch Logs\. In addition to debugging, you can use these for progress analysis\.

**Logs**

The following table lists all of the logs provided by Amazon SageMaker\.

**Logs**

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/logging-cloudwatch.html)

**Note**  
1\. The `/aws/sagemaker/NotebookInstances/[LifecycleConfigHook]` log stream is created when you create a notebook instance with a lifecycle configuration\. For more information, see [Customize a Notebook Instance Using a Lifecycle Configuration Script](notebook-lifecycle-config.md)\.  
2\. For Inference Pipelines, if you don't provide container names, the platform uses \*\*container\-1, container\-2\*\*, and so on, corresponding to the order provided in the SageMaker model\.

For more information about logging events with CloudWatch logging, see [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch User Guide*\.