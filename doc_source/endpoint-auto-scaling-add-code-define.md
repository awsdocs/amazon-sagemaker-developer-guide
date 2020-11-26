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