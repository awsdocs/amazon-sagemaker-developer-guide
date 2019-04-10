# Log Amazon SageMaker Events with Amazon CloudWatch<a name="logging-cloudwatch"></a>

To help you debug your training jobs, endpoints, transform jobs, and notebook instance lifecycle configurations, anything an algorithm container, a model container, or a notebook instance lifecycle configuration sends to `stdout` or `stderr` is also sent to Amazon CloudWatch Logs\. In addition to debugging, you can use these for progress analysis\.

**Logs**

The following table lists all of the logs provided by Amazon SageMaker\.

**Logs**

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/logging-cloudwatch.html)

**Note**  
1\. The `/aws/sagemaker/NotebookInstances` log group is created when you create a notebook instance with a lifecycle configuration\. For more information, see [Step 1\.1: \(Optional\) Customize a Notebook Instance ](notebook-lifecycle-config.md)\.  
2\. For Inference Pipelines, if you don't provide container names, the platform uses \*\*container\-1, container\-2\*\*, and so on, corresponding to the order provided in the Amazon SageMaker model\.

For more information on Amazon SageMaker logging, see [Log Amazon SageMaker Events with Amazon CloudWatch](#logging-cloudwatch)\. 