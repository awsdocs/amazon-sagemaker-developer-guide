# Log Amazon SageMaker Events with Amazon CloudWatch<a name="logging-cloudwatch"></a>

To help you debug your training jobs, endpoints, and notebook instance lifecycle configurations, anything an algorithm container, a model container, or a notebook instance lifecycle configuration sends to `stdout` or `stderr` is also sent to Amazon CloudWatch Logs\. In addition to debugging, you can use these for progress analysis\.

**Logs**

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/logging-cloudwatch.html)

**Note**  
The `/aws/sagemaker/NotebookInstances` log group is made when you create a Notebook Instance with a Lifecycle configuration\. For more information, see [Step 1\.1: \(Optional\) Customize a Notebook Instance ](notebook-lifecycle-config.md)\.