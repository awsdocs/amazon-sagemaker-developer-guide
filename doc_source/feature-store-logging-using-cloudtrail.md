# Logging Feature Store operations by using AWS CloudTrail<a name="feature-store-logging-using-cloudtrail"></a>

Amazon SageMaker Feature Store is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Feature Store\. CloudTrail captures all of the API calls for Feature Store listed on this page\. The logged events include API calls from Feature Store resource management and data operations\. When you create a trail, you activate continuous delivery of CloudTrail events from Feature Store to an Amazon S3 bucket\. Using the information collected by CloudTrail, you can determine the request that was made to Feature Store, the IP address from which the request was made, who made the request, when it was made, and additional details\.

To learn more about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide)\.

## Management events<a name="feature-store-logging-using-cloudtrail-management-events"></a>

Management events capture operations performed on Feature Store resources in your AWS account\. For example, the log generated from the management events provides visibility if a user creates or deletes a Feature Store\. The following APIs log management events with Amazon SageMaker Feature Store\.
+ `CreateFeatureGroup`
+ `DeleteFeatureGroup`
+ `DescribeFeatureGroup`
+ `UpdateFeatureGroup`

Amazon SageMaker API calls and management events are logged by default when you create the account, as described in [Log Amazon SageMaker API Calls with AWS CloudTrail](logging-using-cloudtrail.md)\. For more information, see [ Logging management events for trails](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-management-events-with-cloudtrail.html)\. 

## Data events<a name="feature-store-logging-using-cloudtrail-data-events"></a>

Data events capture data plane operations performed using the Feature Store resources in your AWS account\. For example, the log generated from the data events provides visibility if a user adds or deletes a record within a feature group\. The following APIs log data events with Amazon SageMaker Feature Store\. 
+ `BatchGetRecord`
+ `DeleteRecord`
+ `GetRecord`
+ `PutRecord`

Data events are *not* logged by CloudTrail trails by default\. To activate logging of data events, turn on logging of data plane API activity in CloudTrail\. For more information, see CloudTrail's [ Logging data events for trails](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html)\. 

 The following is an example CloudTrail event for a `PutRecord` API call: 

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "USERPRINCIPALID",
        "arn": "arn:aws:iam::123456789012:user/user",
        "accountId": "123456789012",
        "accessKeyId": "USERACCESSKEYID",
        "userName": "your-user-name"
    },
    "eventTime": "2023-01-01T01:00:00Z",
    "eventSource": "sagemaker.amazonaws.com",
    "eventName": "PutRecord",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0",
    "userAgent": "your-user-agent",
    "requestParameters": {
        "featureGroupName": "your-feature-group-name"
    },
    "responseElements": null,
    "requestID": "request-id",
    "eventID": "event-id",
    "readOnly": false,
    "resources": [
        {
            "accountId": "123456789012",
            "type": "AWS::SageMaker::FeatureGroup",
            "ARN": "arn:aws:sagemaker:us-east-1:123456789012:feature-group/your-feature-group-name"
        }
    ],
    "eventType": "AwsApiCall",
    "managementEvent": false,
    "recipientAccountId": "123456789012",
    "eventCategory": "Data",
    "tlsDetails": {
        ...
    }
}
```