# Use Amazon CloudWatch Events in Amazon Augmented AI<a name="augmented-ai-cloudwatch-events"></a>

Amazon Augmented AI uses Amazon CloudWatch Events \(CloudWatch Events\) to alert you when a human review loop changes status\. When a review loop changes to the `Completed`, `Failed`, or `Stopped` status, Augmented AI sends an event to CloudWatch Events similar to the following:

```
{
    "version":"0",
    "id":"12345678-1111-2222-3333-12345EXAMPLE",
    "detail-type":"Amazon Augmented AI HumanLoop Status Change",
    "source":"aws.sagemaker",
    "account":"1111111111111",
    "time":"2019-11-14T17:49:25Z",
    "region":"us-east-1",
    "resources":["arn:aws:sagemaker:us-east-1:111111111111:human-loop/humanloop-nov-14-1"],
    "detail":{
        "creationTime":"2019-11-14T17:37:36.740Z",
        "failureCode":null,
        "failureReason":null,
        "flowDefinitionArn":"arn:aws:sagemaker:us-west-2:111111111111:flow-definition/flowdef-nov-12",
        "humanLoopArn":"arn:aws:sagemaker:us-west-2:111111111111:human-loop/humanloop-nov-14-1",
        "humanLoopName":"humanloop-nov-14-1",
        "humanLoopOutput":{ 
            "outputS3Uri":"s3://path-specified-in-flow-definition/flowdef-nov-12/2019/11/14/humanloop-nov-14-1/20191114T173736Z/output.json"
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

## Use Human Review Output<a name="using-human-review-output"></a>

After you receive human review results, you can analyze the results and compare them to machine learning predictions\. The JSON that is stored in the Amazon S3 bucket contains both the machine learning predictions and the human review results\.

## More Information<a name="amazon-augmented-ai-programmatic-walkthroughs"></a>

[React to Amazon SageMaker Job Status Changes with CloudWatch Events](cloudwatch-events.md)