# Define a scaling policy<a name="endpoint-auto-scaling-add-code-define"></a>

To specify the metrics and target values for a scaling policy, you configure a target\-tracking scaling policy\. You can use either a predefined metric or a custom metric\.

Scaling policy configuration is represented by a JSON block\. You save your scaling policy configuration as a JSON block in a text file\. You use that text file when invoking the AWS CLI or the Application Auto Scaling API\. For more information about policy configuration syntax, see [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the *Application Auto Scaling API Reference*\.

 The following options are available for defining a target\-tracking scaling policy configuration\.

## Use a predefined metric<a name="endpoint-auto-scaling-add-code-predefined"></a>

To quickly define a target\-tracking scaling policy for a variant, use the `SageMakerVariantInvocationsPerInstance`  predefined metric\. `SageMakerVariantInvocationsPerInstance` is the average number of times per minute that each instance for a variant is invoked\. We strongly recommend using this metric\.



To use a predefined metric in a scaling policy, create a target tracking configuration for your policy\. In the target tracking configuration, include a `PredefinedMetricSpecification` for the predefined metric and a `TargetValue` for the target value of that metric\.

**Example**  
The following example is a typical policy configuration for target\-tracking scaling for a variant\. In this configuration, we use the `SageMakerVariantInvocationsPerInstance` predefined metric to adjust the number of variant instances so that each instance has a `InvocationsPerInstance` metric of 70\.   

```
{
    "TargetValue": 70.0,
    "PredefinedMetricSpecification":
    {
        "PredefinedMetricType": "SageMakerVariantInvocationsPerInstance"
    }
}
```

## Use a custom metric<a name="endpoint-auto-scaling-add-code-custom"></a>

If you need to define a target\-tracking scaling policy that meets your custom requirements, define a custom metric\. You can define a custom metric based on any production variant metric that changes in proportion to scaling\. 

Not all SageMaker metrics work for target tracking\. The metric must be a valid utilization metric, and it must describe how busy an instance is\. The value of the metric must increase or decrease in inverse proportion to the number of variant instances\. That is, the value of the metric should decrease when the number of instances increases\.

**Important**  
Before deploying automatic scaling in production, you must test automatic scaling with your custom metric\.

**Example**  
The following example is a target\-tracking configuration for a scaling policy\. In this configuration, for a variant named `my-variant`, a custom metric adjusts the variant based on an average CPU utilization of 50 percent across all instances\.  

```
{
    "TargetValue": 50,
    "CustomizedMetricSpecification":
    {
        "MetricName": "CPUUtilization",
        "Namespace": "/aws/sagemaker/Endpoints",
        "Dimensions": [
            {"Name": "EndpointName", "Value": "my-endpoint" },
            {"Name": "VariantName","Value": "my-variant"}
        ],
        "Statistic": "Average",
        "Unit": "Percent"
    }
}
```

## Use an online explainability metric<a name="endpoint-auto-scaling-online-explainability"></a>

This guide shows how to use a metric to track the scaling of endpoints with online explainability\. When an endpoint has online explainability activated, it emits a `ExplanationsPerInstance` metric that outputs the average number of records explained per minute, per instance, for a [variant](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html)\. The resource utilization of explaining records can be more different than that of predicting records\. We strongly recommend using this metric for target\-tracking scaling of endpoints with online explainability activated\.

To use this online explainability metric inside a scaling policy, create a target tracking configuration\. In the tracking configuration, include a `CustomizedMetricSpecification` for the custom metrics and a `TargetValue` for the target value\.

The following is an example of a policy configuration that uses `ExplanationsPerInstance`\. It is used to adjust the number of variant instances so that each instance has an `ExplanationsPerInstance` metric of 20\.

```
{
    "TargetValue": 20,
    "CustomizedMetricSpecification":
    {
        "MetricName": "ExplanationsPerInstance",
        "Namespace": "AWS/SageMaker",
        "Dimensions": [
            {"Name": "EndpointName", "Value": "my-endpoint" },
            {"Name": "VariantName","Value": "my-variant"}
        ],
        "Statistic": "Sum"
    }
}
```

[Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-target-tracking.html) allows multiple target tracking scaling policies for a scalable target\. Consider adding the `InvocationsPerInstance` policy from the [predefined metric](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling-add-code-define.html#endpoint-auto-scaling-add-code-predefined) section \(in addition to the `ExplanationsPerInstance` policy\)\. For example, if most invocations don't return an explanation because of the threshold value set in the `EnableExplanations` parameter, then the endpoint can choose the `InvocationsPerInstance` policy\. If there is a large number of explanations, the endpoint can use the `ExplanationsPerInstance` policy\. 

## Add a cooldown period<a name="endpoint-auto-scaling-add-code-cooldown"></a>

To add a cooldown period for scaling\-out your model, specify a value, in seconds, for `ScaleOutCooldown`\. Similarly, to add a cooldown period for scaling\-in your model, add a value, in seconds, for `ScaleInCooldown`\. For more information about `ScaleInCooldown` and `ScaleOutCooldown`, see [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the *Application Auto Scaling API Reference*\. 

**Example**  
The following is an example target\-tracking configuration for a scaling policy\. In this configuration, the `SageMakerVariantInvocationsPerInstance` predefined metric is used to adjust scaling based on an average of 70 across all instances of that variant\. The configuration provides a scale\-in cooldown period of 10 minutes and a scale\-out cooldown period of 5 minutes\.   

```
{
    "TargetValue": 70.0,
    "PredefinedMetricSpecification":
    {
        "PredefinedMetricType": "SageMakerVariantInvocationsPerInstance"
    },
    "ScaleInCooldown": 600,
    "ScaleOutCooldown": 300
}
```