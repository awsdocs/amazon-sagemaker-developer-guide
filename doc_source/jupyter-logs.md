# Monitor Jupyter Logs in Amazon CloudWatch Logs<a name="jupyter-logs"></a>

Jupyter logs include important information such as events, metrics, and health information that provide actionable insights when running Amazon SageMaker notebooks\. By importing Jupyter logs into CloudWatch Logs, customers can use CloudWatch Logs to detect anomalous behaviors, set alarms, and discover insights to keep the SageMaker notebooks running more smoothly\. You can access the logs even when the Amazon EC2 instance that hosts the notebook is unresponsive, and use the logs to troubleshoot the unresponsive notebook\. Sensitive information such as AWS account IDs, secret keys, and authentication tokens in presigned URLs are removed so that customers can share logs without leaking private information\. 

**To view Jupyter logs for a notebook instance:**

1. Sign in to the AWS Management Console and open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. Choose **Notebook instances**\.

1. In the list of notebook instances, choose the notebook instance for which you want to view Jupyter logs\.

1. Under **Monitor** on the notebook instance details page, choose **View logs**\.

1. In the CloudWatch console, choose the log stream for your notebook instance\. Its name is in the form `NotebookInstanceName/jupyter.log`\.

For more information about monitoring CloudWatch logs for SageMaker, see [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\.