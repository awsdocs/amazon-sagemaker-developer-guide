# Register a model<a name="endpoint-auto-scaling-add-policy"></a>

You can add autoscaling for a model with the AWS CLI or the Application Auto Scaling API\. You first must register the model, then you must define an autoscaling policy\.

## Register a model with the AWS CLI<a name="endpoint-auto-scaling-add-cli"></a>

With the AWS CLI, you can configure autoscaling based on either a predefined or a custom metric\.

To register your endpoint, use the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) AWS CLI command with the following parameters:
+ `--service-namespace`—Set this value to `sagemaker`\.
+ `--resource-id`—The resource identifier for the model \(specifically, the production variant\)\. For this parameter, the resource type is `endpoint` and the unique identifier is the name of the production variant\. For example, `endpoint/MyEndpoint/variant/MyVariant`\.
+ `--scalable-dimension`—Set this value to `sagemaker:variant:DesiredInstanceCount`\.
+ `--min-capacity`—The minimum number of instances that for this model\. Set `min-capacity` to at least 1\. It must be equal to or less than the value specified for `max-capacity`\.
+ `--max-capacity`—The maximum number of instances that Application Auto Scaling should manage\. Set `max-capacity` to a minimum of 1, It must be equal to or greater than the value specified for `min-capacity`\.

**Example**  
The following example shows how to register a model named `MyVariant` that is dynamically scaled to have one to eight instances:  

```
aws application-autoscaling register-scalable-target \
--service-namespace sagemaker \
--resource-id endpoint/MyEndPoint/variant/MyVariant \
--scalable-dimension sagemaker:variant:DesiredInstanceCount \
--min-capacity 1 \
--max-capacity 8
```

## Register a model with the Application Auto Scaling API<a name="endpoint-auto-scaling-add-api"></a>

To define the scaling limits for the model, register your model with Application Auto Scaling\. Application Auto Scaling dynamically scales the number of production variant instances\.

To register your model with Application Auto Scaling, use the [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) Application Auto Scaling API action with the following parameters:
+ `ServiceNamespace`—Set this value to `sagemaker`\.
+ `ResourceID`—The resource identifier for the production variant\. For this parameter, the resource type is `endpoint` and the unique identifier is the name of the variant, for example `endpoint/MyEndPoint/variant/MyVariant`\.
+ `ScalableDimension`—Set this value to `sagemaker:variant:DesiredInstanceCount`\.
+ `MinCapacity`—The minimum number of instances to be managed by Application Auto Scaling\. This value must be set to at least 1 and must be equal to or less than the value specified for `MaxCapacity`\.
+ `MaxCapacity`—The maximum number of instances to be managed by Application Auto Scaling\. This value must be set to at least 1 and must be equal to or greater than the value specified for `MinCapacity`\.

**Example**  
The following example shows how to register a SageMaker production variant that is dynamically scaled to use one to eight instances:   

```
POST / HTTP/1.1
    Host: autoscaling.us-east-2.amazonaws.com
    Accept-Encoding: identity
    X-Amz-Target: AnyScaleFrontendService.RegisterScalableTarget
    X-Amz-Date: 20160506T182145Z
    User-Agent: aws-cli/1.10.23 Python/2.7.11 Darwin/15.4.0 botocore/1.4.8
    Content-Type: application/x-amz-json-1.1
    Authorization: AUTHPARAMS

    {
        "ServiceNamespace": "sagemaker",
        "ResourceId": "endpoint/MyEndPoint/variant/MyVariant",
        "ScalableDimension": "sagemaker:variant:DesiredInstanceCount",
        "MinCapacity": 1,
        "MaxCapacity": 8
    }
```