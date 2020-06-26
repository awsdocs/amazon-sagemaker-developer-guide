# Monitor Labeling Job Status<a name="sms-monitor-cloud-watch"></a>

To monitor the status of your labeling jobs, you can set up an [Amazon CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) \(CloudWatch Events\) rule for Amazon SageMaker Ground Truth \(Ground Truth\) to send an event to CloudWatch Events when a labeling job status changes to `Completed`, `Failed`, or `Stopped`\. 

Once you create a rule, you can add a *target* to it\. CloudWatch Events uses this target to invoke another AWS service to process the event\. For example, you can create a target using a Amazon Simple Notification Service \(Amazon SNS\) topic to send a notification to your email when a labeling job status changes\.

**Prerequisites:**

To create a CloudWatch Events rule, you will need an AWS Identity and Access Management \(IAM\) role with an events\.amazonaws\.com trust policy attached\. The following is an example of an events\.amazonaws\.com trust policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "events.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

**Topics**
+ [Send Events to CloudWatch Events](#sms-cloud-watch-event-rule-setup)
+ [Set Up a Target to Process Events](#sms-cloud-watch-events-labelingjob-notifications)
+ [Labeling Job Expiration](#sms-labeling-job-expiration)

## Send Events to CloudWatch Events<a name="sms-cloud-watch-event-rule-setup"></a>

To configure a CloudWatch Events rule to get status updates, or *events*, for your Ground Truth labeling jobs, use the AWS Command Line Interface \(AWS CLI\) [https://docs.aws.amazon.com/cli/latest/reference/events/put-rule.html](https://docs.aws.amazon.com/cli/latest/reference/events/put-rule.html) command\. You can filter events that are sent to your rule by status change\. For example, you can create a rule that notifies you only if a labeling job status changes to `Completed`\. When using the `put-rule` command, specify the following to receive labeling job statuses: 
+ `\"source\":[\"aws.sagemaker\"]`
+ `\"detail-type\":[\"SageMaker Ground Truth Labeling Job State Change\"]`

To configure a CloudWatch Events rule to watch for all status changes, use the following command and replace the placeholder text\. For example, replace `"GTLabelingJobStateChanges"` with a unique CloudWatch Events rule name and *`"arn:aws:iam::111122223333:role/MyRoleForThisRule"`* with the Amazon Resource Number \(ARN\) of an IAM role with an events\.amazonaws\.com trust policy attached\. 

```
aws events put-rule --name "GTLabelingJobStateChanges" 
    --event-pattern "{\"source\":[\"aws.sagemaker\"],\"detail-type\":[\"SageMaker Ground Truth Labeling Job State Change\"]}" 
    --role-arn "arn:aws:iam::111122223333:role/MyRoleForThisRule" 
    --region "region"
```

To filter by job status, use the `\"detail\":{\"LabelingJobStatus\":[\"Status\"]}}"` syntax\. Valid values for `Status` are `Completed`, `Failed`, and `Stopped`\. 

The following example creates a CloudWatch Events rule that notifies you when a labeling job in us\-west\-2 \(Oregon\) changes to `Completed`\.

```
aws events put-rule --name "LabelingJobCompleted" 
    --event-pattern "{\"source\":[\"aws.sagemaker\"],\"detail-type\":[\"SageMaker Ground Truth Labeling Job State Change\"], \"detail\":{\"LabelingJobStatus\":[\"Completed\"]}}"  
    --role-arn "arn:aws:iam::111122223333:role/MyRoleForThisRule" 
    --region us-west-2
```

The following example creates a CloudWatch Events rule that notifies you when a labeling job in us\-east\-1 \(Virginia\) changes to `Completed` or `Failed`\.

```
aws events put-rule --name "LabelingJobCompletedOrFailed" 
    --event-pattern "{\"source\":[\"aws.sagemaker\"],\"detail-type\":[\"SageMaker Ground Truth Labeling Job State Change\"], \"detail\":{\"LabelingJobStatus\":[\"Completed\", \"Failed\"]}}"  
    --role-arn "arn:aws:iam::111122223333:role/MyRoleForThisRule" 
    --region us-east-1
```

 To learn more about the `put-rule` request, see [Event Patterns in CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.

## Set Up a Target to Process Events<a name="sms-cloud-watch-events-labelingjob-notifications"></a>

After you have created a rule, events similar to the following are sent to CloudWatch Events\. In this example, the labeling job `test-labeling-job`'s status changed to `Completed`\.

```
{
    "version": "0",
    "id": "111e1111-11d1-111f-b111-1111b11dcb11",
    "detail-type": "SageMaker Ground Truth Labeling Job State Change",
    "source": "aws.sagemaker",
    "account": "111122223333",
    "time": "2018-10-06T12:26:13Z",
    "region": "us-east-1",
    "resources": [
        "arn:aws:sagemaker:us-east-1:111122223333:labeling-job/test-labeling-job"
    ],
    "detail": {      
        "LabelingJobStatus": "Completed"
    }
}
```

To process events, you need to set up a target\. For example, if you want to receive an email when your labeling job status changes, use a procedure in [Setting Up Amazon SNS Notifications](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_SetupSNS.html) in the *Amazon CloudWatch User Guide *to set up an Amazon SNS topic and subscribe your email to it\. Once you have create a topic, you can use it to create a target\. 

**To add a target to your CloudWatch Events rule**

1. Open the CloudWatch console: [https://console.aws.amazon.com/cloudwatch/home](https://console.aws.amazon.com/cloudwatch/home)

1. In the navigation pane, choose **Rules**\.

1. Choose the rule that you want to add a target to\. 

1. Choose **Actions**, and then choose **Edit**\.

1. Under **Targets**, choose **Add Target** and choose the AWS service you want to act when a labeling job status change event is detected\. 

1. Configure your target\. For instructions, see the topic for configuring a target in the [AWS documentation for that service](https://docs.aws.amazon.com/index.html)\.

1. Choose **Configure details**\.

1. For **Name**, enter a name and, optionally, provide details about the purpose of the rule in **Description**\. 

1. Make sure that the check box next to **State** is selected so that your rule is listed as **Enabled**\. 

1. Choose **Update rule**\.

## Labeling Job Expiration<a name="sms-labeling-job-expiration"></a>

If your labeling job is not completed after 30 days, it will expire\. If your labeling job expires, you can chain the job to create a new labeling job that will only send unlabeled data to workers\. For more information, and to learn how to create a labeling job using chaining, see [Chaining Labeling Jobs](sms-reusing-data.md)\.