# Logging and Monitoring<a name="sagemaker-incident-response"></a>

You can monitor Amazon SageMaker using Amazon CloudWatch, which collects raw data and processes it into readable, near real\-time metrics\. These statistics are kept for 15 months, so that you can access historical information and gain a better perspective on how your web application or service is performing\. You can also set alarms that watch for certain thresholds and send notifications or take actions when those thresholds are met\. For more information, see [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md)\.

Amazon CloudWatch Logs enables you to monitor, store, and access your log files from Amazon EC2 instances, AWS CloudTrail, and other sources\. You can collect and track metrics, create customized dashboards, and set alarms that notify you or take actions when a specified metric reaches a threshold that you specify\. CloudWatch Logs can monitor information in the log files and notify you when certain thresholds are met\. You can also archive your log data in highly durable storage\. For more information, see [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\.

AWS CloudTrail provides a record of actions taken by a user, role, or an AWS service in SageMaker\. Using the information collected by CloudTrail, you can determine the request that was made to SageMaker, the IP address from which the request was made, who made the request, when it was made, and additional details\. For more information, [Log Amazon SageMaker API Calls with AWS CloudTrail](logging-using-cloudtrail.md)\.

**Note**  
CloudTrail does not monitor calls to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html)\.

You can create rules in Amazon CloudWatch Events to react to status changes in status in a SageMaker training, hyperperparameter tuning, or batch transform job\. For more information, see [Automating Amazon SageMaker with Amazon EventBridge](automating-sagemaker-with-eventbridge.md)\.