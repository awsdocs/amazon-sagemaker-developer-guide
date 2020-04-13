# Apply a scaling policy<a name="endpoint-auto-scaling-add-code-apply"></a>

After registering your model and defining a scaling policy, apply the scaling policy to the registered model\. To apply a scaling policy, you can use the AWS CLI or the Application Auto Scaling API\. 

## Apply a scaling policy \(AWS CLI\)<a name="endpoint-auto-scaling-add-code-apply-api"></a>

To apply a scaling policy to your model, use the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) AWS CLI command with the following parameters:
+ `--policy-name`—The name of the scaling policy\.
+ `--policy-type`—Set this value to `TargetTrackingScaling`\.
+ `--resource-id`—The resource identifier for the variant\. For this parameter, the resource type is `endpoint` and the unique identifier is the name of the variant\. For example `endpoint/MyEndpoint/variant/MyVariant`\.
+ `--service-namespace`—Set this value to `sagemaker`\.
+ `--scalable-dimension`—Set this value to `sagemaker:variant:DesiredInstanceCount`\.
+ `--target-tracking-scaling-policy-configuration`—The target\-tracking scaling policy configuration to use for the model\.

**Example**  
The following example uses with Application Auto Scaling to apply a target\-tracking scaling policy named `myscalablepolicy` to a model \(variant\) named `myscalablevariant`\. The policy configuration is saved in a file named `config.json`\.  

```
aws application-autoscaling put-scaling-policy \
    --policy-name myscalablepolicy \
    --policy-type TargetTrackingScaling \
    --resource-id endpoint/MyEndpoint/variant/MyVariant \
    --service-namespace sagemaker \
    --scalable-dimension sagemaker:variant:DesiredInstanceCount \
    --target-tracking-scaling-policy-configuration file://config.json
```

## Apply a scaling policy \(Application Auto Scaling API\)<a name="endpoint-auto-scaling-add-code-apply-api"></a>

To apply a scaling policy to a variant with the Application Auto Scaling API, use the [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_PutScalingPolicy.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_PutScalingPolicy.html) Application Auto Scaling API action with the following parameters:
+ `PolicyName`—The name of the scaling policy\.
+ `ServiceNamespace`—Set this value to `sagemaker`\.
+ `ResourceID`—The resource identifier for the variant\. For this parameter, the resource type is `endpoint` and the unique identifier is the name of the variant\. For example, `endpoint/MyEndpoint/variant/MyVariant`\.
+ `ScalableDimension`—Set this value to `sagemaker:variant:DesiredInstanceCount`\.
+ `PolicyType`—Set this value to `TargetTrackingScaling`\.
+ `TargetTrackingScalingPolicyConfiguration`—The target\-tracking scaling policy configuration to use for the variant\.

**Example**  
The following example uses Application Auto Scaling to apply a target\-tracking scaling policy named `myscalablepolicy` to a variant named `myscalablevariant`\. It uses a policy configuration based on the `SageMakerVariantInvocationsPerInstance` predefined metric\.  

```
POST / HTTP/1.1
Host: autoscaling.us-east-2.amazonaws.com
Accept-Encoding: identity
X-Amz-Target: AnyScaleFrontendService.
X-Amz-Date: 20160506T182145Z
User-Agent: aws-cli/1.10.23 Python/2.7.11 Darwin/15.4.0 botocore/1.4.8
Content-Type: application/x-amz-json-1.1
Authorization: AUTHPARAMS

{
    "PolicyName": "myscalablepolicy",
    "ServiceNamespace": "sagemaker",
    "ResourceId": "endpoint/MyEndpoint/variant/MyVariant",
    "ScalableDimension": "sagemaker:variant:DesiredInstanceCount",
    "PolicyType": "TargetTrackingScaling",
    "TargetTrackingScalingPolicyConfiguration": {
        "TargetValue": 70.0,
        "PredefinedMetricSpecification":
        {
            "PredefinedMetricType": "SageMakerVariantInvocationsPerInstance"
        }
    }
}
```