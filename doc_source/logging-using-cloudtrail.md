# Logging Amazon SageMaker API Calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon SageMaker is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon SageMaker\. By creating a *trail*, a configuration that enables delivery of events as log files to an Amazon Simple Storage Service \(Amazon S3\) bucket, you can enable continuous delivery of CloudTrail events to an S3 bucket, Amazon CloudWatch Logs, and Amazon CloudWatch Events\.  Use the information collected by CloudTrail to determine the request that was made to Amazon SageMaker, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon SageMaker Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

All Amazon SageMaker actions except the Amazon SageMaker Runtime action `InvokeEndpoint` are logged by CloudTrail \. For example, calls to the `CreateTrainingJob`, `CreateModel`, and `CreateEndpoint` actions generate entries in the CloudTrail log files\. 

Every event or log entry contains information about who generated the request\. This information helps you determine the following: 

+ Whether the request was made with root or IAM user credentials\.

+ Whether the request was made with temporary security credentials for a role or federated user\.

+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

 When you create a trail, you can store your log files in your S3 bucket for as long as you want\. You can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\.

You can aggregate Amazon SageMaker log files from multiple AWS Regions and multiple AWS accounts into a single S3 bucket\. For more information, see [Receiving CloudTrail Log Files from Multiple Regions](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)\.

To be notified of log file delivery, configure CloudTrail to publish Amazon Simple Notification Service \(Amazon SNS\) notifications\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

## Understanding Amazon SageMaker Log File Entries<a name="understanding-service-name-entries"></a>

A trail is a configuration that enables delivery of events as log files to an S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files are not an ordered stack trace of the public API calls, so they do not appear in any specific order\. 

The following examples a log entry for the `CreateEndpoint` action, which creates an endpoint to deploy a trained model\.

```
{
    "eventVersion":"1.05",
    "userIdentity": {
        "type":"IAMUser",
        "principalId":"AIXDAYQEXAMPLEUMLYNGL",
        "arn":"arn:aws:iam::123456789012:user/intern",
        "accountId":"123456789012",
        "accessKeyId":"ASXIAGXEXAMPLEQULKNXV",
        "userName":"intern"
    },
    "eventTime":"2018-01-02T13:39:06Z",
    "eventSource":"sagemaker.amazonaws.com",
    "eventName":"CreateEndpoint",
    "awsRegion":"us-west-2",
    "sourceIPAddress":"127.0.0.1",
    "userAgent":"USER_AGENT",
    "requestParameters": {
        "endpointName":"ExampleEndpoint",
        "endpointConfigName":"ExampleEndpointConfig"
    },
    "responseElements": {
        "endpointArn":"arn:aws:sagemaker:us-west-2:123456789012:endpoint/exampleendpoint"
    },
    "requestID":"6b1b42b9-EXAMPLE",
    "eventID":"a6f85b21-EXAMPLE",
    "eventType":"AwsApiCall",
    "recipientAccountId":"444455556666"
}
```

The following example is a log entry for the `CreateModel` action, which creates one or more containers to host a previously trained model\.

```
{
    "eventVersion":"1.05",
    "userIdentity": {
        "type":"IAMUser",
        "principalId":"AIXDAYQEXAMPLEUMLYNGL",
        "arn":"arn:aws:iam::123456789012:user/intern",
        "accountId":"123456789012",
        "accessKeyId":"ASXIAGXEXAMPLEQULKNXV",
        "userName":"intern"
    },
    "eventTime":"2018-01-02T15:23:46Z",
    "eventSource":"sagemaker.amazonaws.com",
    "eventName":"CreateModel",
    "awsRegion":"us-west-2",
    "sourceIPAddress":"127.0.0.1",
    "userAgent":"USER_AGENT",
    "requestParameters": {
        "modelName":"ExampleModel",
        "primaryContainer": {
            "image":"174872318107.dkr.ecr.us-west-2.amazonaws.com/kmeans:latest"
        },
        "executionRoleArn":"arn:aws:iam::123456789012:role/EXAMPLEARN"
    },
    "responseElements": {
        "modelArn":"arn:aws:sagemaker:us-west-2:123456789012:model/barkinghappy2018-01-02t15-23-32-275z-ivrdog"
    },
    "requestID":"417b8dab-EXAMPLE",
    "eventID":"0f2b3e81-EXAMPLE",
    "eventType":"AwsApiCall",
    "recipientAccountId":"444455556666"
}
```