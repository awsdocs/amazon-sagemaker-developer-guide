# Monitor experiment training metrics with AWS CloudTrail<a name="experiments-monitoring"></a>

The training metrics for Amazon SageMaker Experiments are integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service\. CloudTrail captures all API calls for `[BatchPutMetrics](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_metrics_BatchPutMetrics.html)` as events\. SageMaker automatically calls `BatchPutMetrics` when you [create an experiment run using the SageMaker SDK for Python](https://docs.aws.amazon.com/sagemaker/latest/dg/experiments-create.html#experiments-create-python-sdk)\. AWS CloudTrail captures data related to calls for resource type `AWS::SageMaker::ExperimentTrialComponent`\. 

**Note**  
In the Studio Experiments UI, trials are referred to as* run groups* and trial components are referred to as *runs*\.

When you [create an experiment run](https://docs.aws.amazon.com/sagemaker/latest/dg/experiments-create.html), you can also configure the continuous delivery of CloudTrail events to an Amazon S3 bucket\. Use CloudTrail to monitor all ingested training metrics for an experiment run, including information such as the metric name, the training step of the recorded metric, the timestamp, and the metric value\. CloudTrail events also include the experiment run ARN, the ID of the account that created the run, and the resource type, which should be `AWS::SageMaker::ExperimentTrialComponent`\. 

To monitor `BatchPutMetrics` API calls as CloudTrail events, you must first set up the logging of data plane API activity in CloudTrail\. See [Logging data events for trails](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html) for more information\. For granular control over which API calls you want to selectively log and pay for, you can filter CloudTrail events by resource type\. Specify `AWS::SageMaker::ExperimentTrialComponent` as a resource type to monitor calls to the `BatchPutMetrics` API\. For more information, see [DataResource](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_DataResource.html) in the [AWS CloudTrail API reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\. To learn more about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\. 

For an in\-depth explanation of how Amazon SageMaker works with AWS CloudTrail, see [Log Amazon SageMaker API Calls with AWS CloudTrail](logging-using-cloudtrail.md)\.

The following is an example CloudTrail event for a training metric in an experiment run:

```
{
  ...
  "eventTime": "2022-12-14T21:53:41Z",
  "eventSource": "metrics-sagemaker.amazonaws.com",
  "eventName": "BatchPutMetrics",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "192.0.2.0",
  "userAgent": "aws-cli/2.7.25 Python/3.9.11 Linux/5.4.214-134.408.amzn2int.x86_64 exe/x86_64.amzn.2 prompt/off command/sm-metrics.batch-put-metrics",
  "requestParameters": {
    "trialComponentName": "trial-component-name",
    "metricData": [
      {
        "metricName": "foo",
        "timestamp": 1670366870000,
        "step": 101,
        "value": 0.9
      }
    ]
  },
  ...
  "resources": [
    {
      "accountId": "abcdef01234567890",
      "type": "AWS::SageMaker::ExperimentTrialComponent",
      "ARN": "arn:aws:sagemaker:us-east-1:1234567890abcdef0:experiment-trial-component/trial-component-name"
    }
  ],
  ...
}
```