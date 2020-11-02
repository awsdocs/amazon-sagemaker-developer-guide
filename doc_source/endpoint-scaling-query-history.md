# Query Endpoint Autoscaling History<a name="endpoint-scaling-query-history"></a>

You can view the status of scaling activities from your endpoint using [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DescribeScalingActivities.html#API_DescribeScalingActivities_RequestSyntax](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DescribeScalingActivities.html#API_DescribeScalingActivities_RequestSyntax)\. `DescribeScalingActivities` provides descriptive information about the scaling activities in the specified namespace from the previous six weeks\. 

## How To Query Endpoint Autoscaling Actions<a name="endpoint-how-to"></a>

Query your autoscaling endpoints with `DescribeScalingActivities`\. To do so, specify [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DescribeScalingActivities.html#autoscaling-DescribeScalingActivities-request-ServiceNamespace](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DescribeScalingActivities.html#autoscaling-DescribeScalingActivities-request-ServiceNamespace) parameter\. `ServiceNameSpace` is the name of the AWS service that provides the resource\. 

 Valid service name values include the following: 

` ecs | elasticmapreduce | ec2 | appstream | dynamodb | rds | sagemaker | custom-resource | comprehend | lambda | cassandra`

In this situation you need to set `ServiceNameSpace` to `sagemaker`\.

Use the following AWS CLI command to view details about all of your `sagemaker` endpoints that have a scaling policy:

```
aws application-autoscaling describe-scaling-activities \ 
    --service-namespace sagemaker
```

You can search for a specific endpoint using [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DescribeScalingActivities.html#autoscaling-DescribeScalingActivities-request-ResourceId](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DescribeScalingActivities.html#autoscaling-DescribeScalingActivities-request-ResourceId):

```
aws application-autoscaling describe-scaling-activities \
    --service-namespace sagemaker \
    --resource-id endpoint/<endpoint_name>/variant/<variant_name>
```

When you run this command, it returns the following output:

```
{
    "ActivityId": "activity-id",
    "ServiceNamespace": "sagemaker",
    "ResourceId": "endpoint/<endpoint_name>/variant/<variant_name>",
    "ScalableDimension": "sagemaker:variant:DesiredInstanceCount",
    "Description": "string",
    "Cause": "string",
    "StartTime": timestamp,
    "EndTime": timestamp,
    "StatusCode": "string",
    "StatusMessage": "string"
}
```

## How to Identify Blocked AutoScaling Due to Instance Quotas<a name="endpoint-identify-blocked-autoscaling"></a>

When you scale out or add more instances, you might reach your account\-level instance quota\. You can use `DescribeScalingActivities` to check whether you have reached your instance quota\. When you exceed your quota, automatic scaling is blocked\. 

To check if you have reached your instance quota, use the AWS CLI command as shown in the proceeding example where you specified the `ResourceId`:

```
aws application-autoscaling describe-scaling-activities \
    --service-namespace sagemaker \
    --resource-id endpoint/<endpoint_name>/variant/<variant_name>
```

Within the return syntax, check the [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_ScalingActivity.html#autoscaling-Type-ScalingActivity-StatusCode](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_ScalingActivity.html#autoscaling-Type-ScalingActivity-StatusCode) and [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_ScalingActivity.html#autoscaling-Type-ScalingActivity-StatusMessage](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_ScalingActivity.html#autoscaling-Type-ScalingActivity-StatusMessage) keys and their associated values\. `StatusCode` returns `Failed`\. Within `StatusMessage` there is a message indicating that the account\-level service quota was reached\. The following is an example of what that message might look like: 

```
{
    "ActivityId": "activity-id",
    "ServiceNamespace": "sagemaker",
    "ResourceId": "endpoint/<endpoint_name>/variant/<variant_name>",
    "ScalableDimension": "sagemaker:variant:DesiredInstanceCount",
    "Description": "string",
    "Cause": "minimum capacity was set to 110",
    "StartTime": timestamp,
    "EndTime": timestamp,
    "StatusCode": "Failed",
    "StatusMessage": "Failed to set desired instance count to 110. Reason: The 
    account-level service limit 'ml.xx.xxxxxx for endpoint usage' is 1000 
    Instances, with current utilization of 997 Instances and a request delta 
    of 20 Instances. Please contact AWS support to request an increase for this 
    limit. (Service: AmazonSageMaker; Status Code: 400; 
    Error Code: ResourceLimitExceeded; Request ID: request-id)."
}
```