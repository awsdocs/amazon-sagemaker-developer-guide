# Edit a scaling policy<a name="endpoint-auto-scaling-edit"></a>

You can edit an autoscaling policy with the AWS Management Console, the AWS CLI, or the Application Auto Scaling API\. 

## Scale\-in<a name="endpoint-auto-scaling-edit-scale-in"></a>

Scaling\-in occurs when there is no traffic: if a variant’s traffic becomes zero, SageMaker automatically scales in to the minimum number of instances specified\. In this case, SageMaker emits metrics with a value of zero\. Minimum instance count is required to be 1 or higher\.

### Disable scale\-in activity<a name="endpoint-auto-scaling-edit-scale-in-disable"></a>

You can prevent the target\-tracking scaling policy configuration from scaling in your variant by disabling scale\-in activity\. Disabling scale\-in activity prevents the scaling policy from deleting instances, while still allowing it to create them as needed\.

To enable or disable scale\-in activity for your model, specify a Boolean value for `DisableScaleIn`\. For more information about `DisableScaleIn`, see [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the *Application Auto Scaling API Reference*\. 

**Example**  
The following is an example of a target\-tracking configuration for a scaling policy where it will scale\-out, but not scale\-in\. In this configuration, the `SageMakerVariantInvocationsPerInstance` predefined metric will scale\-out based on an average of 70 invocations \(inference requests\) across all instances the model is on\. The configuration also disables scale\-in activity for the scaling policy\.  

```
{
    "TargetValue": 70.0,
    "PredefinedMetricSpecification":
    {
        "PredefinedMetricType": "SageMakerVariantInvocationsPerInstance"
    },
    "DisableScaleIn": true
}
```

## Scale\-out<a name="endpoint-auto-scaling-edit-scale-out"></a>

To manually scale\-out, adjust the minimum capacity\. You can use the console to update this value\. Alternatively, use the AWS CLI with the `--min-capacity` argument, or use the Application Auto Scaling API's `MinCapacity` parameter\.

### Disable scale\-out activity<a name="endpoint-auto-scaling-edit-scale-out-disable"></a>

To prevent scale\-out, adjust the maximum capacity\. You can use the console to update this value\. Alternatively, use the AWS CLI with the `--max-capacity` argument, or use the Application Auto Scaling API's `MaxCapacity` parameter\.

## Edit a scaling policy \(Console\)<a name="endpoint-auto-scaling-edit-console"></a>

 To edit a scaling policy with the AWS Management Console, use the same procedure that you used to [Configure model autoscaling with the console](endpoint-auto-scaling-add-console.md)\.

## Edit a scaling policy \(AWS CLI or Application Auto Scaling API\)<a name="endpoint-auto-scaling-edit-code"></a>

You can use the AWS CLI or the Application Auto Scaling API to edit a scaling policy in the same way that you apply a scaling policy:
+ With the AWS CLI, specify the name of the policy that you want to edit in the `--policy-name` parameter\. Specify new values for the parameters that you want to change\.
+ With the Application Auto Scaling API, specify the name of the policy that you want to edit in the `PolicyName` parameter\. Specify new values for the parameters that you want to change\.

For more information, see [Apply a scaling policy](endpoint-auto-scaling-add-code-apply.md)\.