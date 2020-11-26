# Use Amazon CloudWatch Events in Amazon Augmented AI<a name="a2i-cloudwatch-events"></a>

Amazon Augmented AI uses Amazon CloudWatch Events \(CloudWatch Events\) to alert you when a human review loop changes status\. When a review loop changes to the `Completed`, `Failed`, or `Stopped` status, Augmented AI sends an event to CloudWatch Events similar to the following:

```
{
    "version":"0",
    "id":"12345678-1111-2222-3333-12345EXAMPLE",
    "detail-type":"SageMaker A2I HumanLoop Status Change",
    "source":"aws.sagemaker",
    "account":"1111111111111",
    "time":"2019-11-14T17:49:25Z",
    "region":"us-east-1",
    "resources":["arn:aws:sagemaker:us-east-1:111111111111:human-loop/humanloop-nov-14-1"],
    "detail":{
        "creationTime":"2019-11-14T17:37:36.740Z",
        "failureCode":null,
        "failureReason":null,
        "flowDefinitionArn":"arn:aws:sagemaker:us-east-1:111111111111:flow-definition/flowdef-nov-12",
        "humanLoopArn":"arn:aws:sagemaker:us-east-1:111111111111:human-loop/humanloop-nov-14-1",
        "humanLoopName":"humanloop-nov-14-1",
        "humanLoopOutput":{ 
            "outputS3Uri":"s3://customer-output-bucket-specified-in-flow-definition/flowdef-nov-12/2019/11/14/17/37/36/humanloop-nov-14-1/output.json"
        },
        "humanLoopStatus":"Completed"
    }
}
```

The details in the JSON output include the following:

`creationTime`  
The timestamp when Augmented AI created the human loop\.

`failureCode`  
A failure code denoting a specific type of failure\.

`failureReason`  
The reason why a human loop has failed\. The failure reason is only returned when the human review loop status is `failed`\.

`flowDefinitionArn`  
The Amazon Resource Name \(ARN\) of the flow definition, or *human review workflow*\.

`humanLoopArn`  
The Amazon Resource Name \(ARN\) of the human loop\.

`humanLoopName`  
The name of the human loop\.

`humanLoopOutput`  
An object containing information about the output of the human loop\.

`outputS3Uri`  
The location of the Amazon S3 object where Augmented AI stores your human loop output\.

`humanLoopStatus`  
The status of the human loop\.

## Send Events from Your Human Loop to CloudWatch Events<a name="a2i-cloud-watch-events-rule-setup"></a>

To configure a CloudWatch Events rule to get status updates, or *events*, for your Amazon A2I human loops, use the AWS Command Line Interface \(AWS CLI\) [https://docs.aws.amazon.com/cli/latest/reference/events/put-rule.html](https://docs.aws.amazon.com/cli/latest/reference/events/put-rule.html) command\. When using the `put-rule` command, specify the following to receive human loop statuses: 
+ `\"source\":[\"aws.sagemaker\"]`
+ `\"detail-type\":[\"SageMaker A2I HumanLoop Status Change\"]`

To configure a CloudWatch Events rule to watch for all status changes, use the following command and replace the placeholder text\. For example, replace `"A2IHumanLoopStatusChanges"` with a unique CloudWatch Events rule name and *`"arn:aws:iam::111122223333:role/MyRoleForThisRule"`* with the Amazon Resource Number \(ARN\) of an IAM role with an events\.amazonaws\.com trust policy attached\. Replace *region* with the AWS Region you want to create the rule in\. 

```
aws events put-rule --name "A2IHumanLoopStatusChanges" 
    --event-pattern "{\"source\":[\"aws.sagemaker\"],\"detail-type\":[\"SageMaker A2I HumanLoop Status Change\"]}" 
    --role-arn "arn:aws:iam::111122223333:role/MyRoleForThisRule" 
    --region "region"
```

 To learn more about the `put-rule` request, see [Event Patterns in CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.

## Set Up a Target to Process Events<a name="a2i-subscribe-cloud-watch-events"></a>

To process events, you need to set up a target\. For example, if you want to receive an email when a human loop status changes, use a procedure in [Setting Up Amazon SNS Notifications](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_SetupSNS.html) in the *Amazon CloudWatch User Guide *to set up an Amazon SNS topic and subscribe your email to it\. Once you have create a topic, you can use it to create a target\. 

**To add a target to your CloudWatch Events rule**

1. Open the CloudWatch console: [https://console.aws.amazon.com/cloudwatch/home](https://console.aws.amazon.com/cloudwatch/home)

1. In the navigation pane, choose **Rules**\.

1. Choose the rule that you want to add a target to\. 

1. Choose **Actions**, and then choose **Edit**\.

1. Under **Targets**, choose **Add Target** and choose the AWS service you want to act when a human loop status change event is detected\. 

1. Configure your target\. For instructions, see the topic for configuring a target in the [AWS documentation for that service](https://docs.aws.amazon.com/index.html)\.

1. Choose **Configure details**\.

1. For **Name**, enter a name and, optionally, provide details about the purpose of the rule in **Description**\. 

1. Make sure that the check box next to **State** is selected so that your rule is listed as **Enabled**\. 

1. Choose **Update rule**\.

## Use Human Review Output<a name="using-human-review-output"></a>

After you receive human review results, you can analyze the results and compare them to machine learning predictions\. The JSON that is stored in the Amazon S3 bucket contains both the machine learning predictions and the human review results\.

## More Information<a name="amazon-augmented-ai-programmatic-walkthroughs"></a>

[Automating Amazon SageMaker with Amazon EventBridge](automating-sagemaker-with-eventbridge.md)