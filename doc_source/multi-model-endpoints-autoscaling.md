# Set Auto Scaling Policies for Multi\-Model Endpoint Deployments<a name="multi-model-endpoints-autoscaling"></a>

SageMaker multi\-model endpoints fully support automatic scaling, which manages replicas of models to ensure models scale based on traffic patterns\. We recommend that you configure your multi\-model endpoint and the size of your instances based on [Instance recommendations for multi\-model endpoint deployments](multi-model-endpoints.md#multi-model-endpoint-instance) and also set up instance based auto scaling for your endpoint\. The invocation rates used to trigger an auto\-scale event are based on the aggregate set of predictions across the full set of models served by the endpoint\. For additional details on setting up endpoint auto scaling, see [Automatically Scale Amazon SageMaker Models](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling.html)\.

You can set up auto scaling policies with predefined and custom metrics on both CPU and GPU backed multi\-model endpoints\.

**Note**  
SageMaker multi\-model endpoint metrics are available at one\-minute granularity\.

## Define a scaling policy<a name="multi-model-endpoints-autoscaling-define"></a>

To specify the metrics and target values for a scaling policy, you can configure a target\-tracking scaling policy\. You can use either a predefined metric or a custom metric\.

Scaling policy configuration is represented by a JSON block\. You save your scaling policy configuration as a JSON block in a text file\. You use that text file when invoking the AWS CLI or the Application Auto Scaling API\. For more information about policy configuration syntax, see `[TargetTrackingScalingPolicyConfiguration](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html)` in the *Application Auto Scaling API Reference*\.

The following options are available for defining a target\-tracking scaling policy configuration\.

### Use a predefined metric<a name="multi-model-endpoints-autoscaling-predefined"></a>

To quickly define a target\-tracking scaling policy for a variant, use the `SageMakerVariantInvocationsPerInstance` predefined metric\. `SageMakerVariantInvocationsPerInstance` is the average number of times per minute that each instance for a variant is invoked\. We strongly recommend using this metric\.

To use a predefined metric in a scaling policy, create a target tracking configuration for your policy\. In the target tracking configuration, include a `PredefinedMetricSpecification` for the predefined metric and a `TargetValue` for the target value of that metric\.

The following example is a typical policy configuration for target\-tracking scaling for a variant\. In this configuration, we use the `SageMakerVariantInvocationsPerInstance` predefined metric to adjust the number of variant instances so that each instance has an `InvocationsPerInstance` metric of `70`\.

```
{"TargetValue": 70.0,
    "PredefinedMetricSpecification":
    {
        "PredefinedMetricType": "InvocationsPerInstance"
    }
}
```

**Note**  
We recommend that you use `InvocationsPerInstance` while using multi\-model endpoints\. The `TargetValue` for this metric depends on your application’s latency requirements\. We also recommend that you load test your endpoints to set up suitable scaling parameter values\. To learn more about load testing and setting up autoscaling for your endpoints, see the blog [Configuring autoscaling inference endpoints in Amazon SageMaker](http://aws.amazon.com/blogs/machine-learning/configuring-autoscaling-inference-endpoints-in-amazon-sagemaker/)\.

### Use a custom metric<a name="multi-model-endpoints-autoscaling-custom"></a>

If you need to define a target\-tracking scaling policy that meets your custom requirements, define a custom metric\. You can define a custom metric based on any production variant metric that changes in proportion to scaling\.

Not all SageMaker metrics work for target tracking\. The metric must be a valid utilization metric, and it must describe how busy an instance is\. The value of the metric must increase or decrease in inverse proportion to the number of variant instances\. That is, the value of the metric should decrease when the number of instances increases\.

**Important**  
Before deploying automatic scaling in production, you must test automatic scaling with your custom metric\.

#### Example custom metric for a CPU backed multi\-model endpoint<a name="multi-model-endpoints-autoscaling-custom-cpu"></a>

The following example is a target\-tracking configuration for a scaling policy\. In this configuration, for a model named `my-model`, a custom metric of `CPUUtilization` adjusts the instance count on the endpoint based on an average CPU utilization of 50% across all instances\.

```
{"TargetValue": 50,
    "CustomizedMetricSpecification":
    {"MetricName": "CPUUtilization",
        "Namespace": "/aws/sagemaker/Endpoints",
        "Dimensions": [
            {"Name": "EndpointName", "Value": "my-endpoint" },
            {"Name": "ModelName","Value": "my-model"}
        ],
        "Statistic": "Average",
        "Unit": "Percent"
    }
}
```

#### Example custom metric for a GPU backed multi\-model endpoint<a name="multi-model-endpoints-autoscaling-custom-gpu"></a>

The following example is a target\-tracking configuration for a scaling policy\. In this configuration, for a model named `my-model`, a custom metric of `GPUUtilization` adjusts the instance count on the endpoint based on an average GPU utilization of 50% across all instances\.

```
{"TargetValue": 50,
    "CustomizedMetricSpecification":
    {"MetricName": "GPUUtilization",
        "Namespace": "/aws/sagemaker/Endpoints",
        "Dimensions": [
            {"Name": "EndpointName", "Value": "my-endpoint" },
            {"Name": "ModelName","Value": "my-model"}
        ],
        "Statistic": "Average",
        "Unit": "Percent"
    }
}
```

## Add a cooldown period<a name="multi-model-endpoints-autoscaling-cooldown"></a>

To add a cooldown period for scaling out your endpoint, specify a value, in seconds, for `ScaleOutCooldown`\. Similarly, to add a cooldown period for scaling in your model, add a value, in seconds, for `ScaleInCooldown`\. For more information about `ScaleInCooldown` and `ScaleOutCooldown`, see `[TargetTrackingScalingPolicyConfiguration](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html)` in the *Application Auto Scaling API Reference*\.

The following is an example target\-tracking configuration for a scaling policy\. In this configuration, the `SageMakerVariantInvocationsPerInstance` predefined metric is used to adjust scaling based on an average of `70` across all instances of that variant\. The configuration provides a scale\-in cooldown period of 10 minutes and a scale\-out cooldown period of 5 minutes\.

```
{"TargetValue": 70.0,
    "PredefinedMetricSpecification":
    {"PredefinedMetricType": "SageMakerVariantInvocationsPerInstance"
    },
    "ScaleInCooldown": 600,
    "ScaleOutCooldown": 300
}
```