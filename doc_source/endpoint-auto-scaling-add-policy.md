# Configure Automatic Scaling for a Variant<a name="endpoint-auto-scaling-add-policy"></a>

You can configure automatic scaling for a variant with the AWS Management Console, the AWS CLI, or the Application Auto Scaling API\.


+ [Configure Automatic Scaling for a Variant \(Console\)](#endpoint-auto-scaling-add-console)
+ [Configure Automatic Scaling for a Variant \(AWS CLI or the Application Auto Scaling API\)](#endpoint-auto-scaling-add-code)

## Configure Automatic Scaling for a Variant \(Console\)<a name="endpoint-auto-scaling-add-console"></a>

**To configure automatic scaling for a variant \(console\)**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, choose **Endpoints**\. 

1. Choose the endpoint that you want to configure\.

1. For **Endpoint runtime settings**, choose the variant that you want to configure\.

1. For **Endpoint runtime settings**, choose **Configure auto scaling**\.

   The **Configure variant automatic scaling** page appears\.

1. For **Minimum capacity**, type the minimum number of instances that you want the scaling policy to maintain\. At least 1 instance is required\.

1. For **Maximum capacity**, type the maximum number of instances that you want the scaling policy to maintain\.

1. For the target value, type the average number of invocations per instance per minute for the variant\. To determine this value, follow the guidelines in [Load Testing](endpoint-scaling-loadtest.md)\.

   Application Auto Scaling adds or removes instances to keep the metric close to the value that you specify\.

1. For **Scale\-in cool down \(seconds\)** and **Scale\-out cool down \(seconds\)**, type the number seconds for each cool down period\. Assuming that the order in the list is based on either most important to less important of first applied to last applied\.

1. Select **Disable scale in** to prevent the scaling policy from deleting variant instances if you want to ensure that your variant scales out to address increased traffic, but are not concerned with removing instances to reduce costs when traffic decreases, disable scale\-in activities\.

   Scale\-out activities are always enabled so that the scaling policy can create endpoint instances as needed\.

1. Choose **Save**\.

This procedure registers a variant as a scalable target with Application Auto Scaling\. When you register a variant, Application Auto Scaling performs validation checks to ensure the following:

+ The variant exists

+ The permissions are sufficient

+ You aren't registering a variant with an instance that is a burstable performance instance
**Note**  
Amazon SageMaker automatic scaling doesn't support automatic scaling for burstable instances, because they already allow for increased capacity under increased workloads\. For information about burstable performance instances, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\.

## Configure Automatic Scaling for a Variant \(AWS CLI or the Application Auto Scaling API\)<a name="endpoint-auto-scaling-add-code"></a>

With the AWS CLI or the Application Auto Scaling API, you can configure automatic scaling based on either a predefined or a custom metric\.

### Registering a Variant<a name="endpoint-auto-scaling-add-code-register"></a>

To define the scaling limits for the variant, register your variant with Application Auto Scaling\. Application Auto Scaling dynamically scales the number of variant instances\. 

To register your variant, you can use either the AWS CLI or the Application Auto Scaling API\. 

When you register a variant, Application Auto Scaling performs validation checks to ensure the following:

+ The variant resource exists

+ The permissions are sufficient

+ You aren't registering a variant with an instance that is a Burstable Performance Instance
**Note**  
Amazon SageMaker automatic scaling doesn't support automatic scaling for burstable instances, because burstable instances already allow for increased capacity under increased workloads\. For information about Burstable Performance Instances, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\.

#### Register a Variant \(AWS CLI\)<a name="endpoint-auto-scaling-add-code-register-cli"></a>

To register your endpoint, use the [http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) AWS CLI command with the following parameters:

+ `--service-namespace`—Set this value to `sagemaker`\.

+ `--resource-id`—The resource identifier for the production variant\. For this parameter, the resource type is `endpoint` and the unique identifier is the name of the variant\. For example `endpoint/MyEndpoint/variant/MyVariant`\.

+ `--scalable-dimension`—Set this value to `sagemaker:variant:DesiredInstanceCount`\.

+ `--min-capacity`—The minimum number of instances that Application Auto Scaling must manage for this endpoint\. Set `min-capacity` to at least 1\. It must be equal to or less than the value specified for `max-capacity`\.

+ `--max-capacity`—The maximum number of instances that Application Auto Scaling should manage\. Set `max-capacity` to a minimum of 1, It must be equal to or greater than the value specified for `min-capacity`\.

**Example**  
The following example shows how to register an endpoint variant named `MyVariant` that is dynamically scaled to have one to eight instances:  

```
aws application-autoscaling register-scalable-target \
    --service-namespace sagemaker \
    --resource-id endpoint/MyEndPoint/variant/MyVariant \
    --scalable-dimension sagemaker:variant:DesiredInstanceCount \
    --min-capacity 1 \
    --max-capacity 8
```

#### Register a Variant \(Application Auto Scaling API\)<a name="endpoint-auto-scaling-add-code-register-api"></a>

To register your endpoint variant with Application Auto Scaling, use the [http://docs.aws.amazon.com//autoscaling/application/APIReference/API_RegisterScalableTarget.html](http://docs.aws.amazon.com//autoscaling/application/APIReference/API_RegisterScalableTarget.html) Application Auto Scaling API action with the following parameters:

+ `ServiceNamespace`—Set this value to `sagemaker`\.

+ `ResourceID`—The resource identifier for the production variant\. For this parameter, the resource type is `endpoint` and the unique identifier is the name of the variant, for example `endpoint/MyEndPoint/variant/MyVariant`\.

+ `ScalableDimension`—Set this value to `sagemaker:variant:DesiredInstanceCount`\.

+ `MinCapacity`—The minimum number of instances to be managed by Application Auto Scaling\. This value must be set to at least 1 and must be equal to or less than the value specified for `MaxCapacity`\.

+ `MaxCapacity`—The maximum number of instances to be managed by Application Auto Scaling\. This value must be set to at least 1 and must be equal to or greater than the value specified for `MinCapacity`\.

**Example**  
The following example shows how to register an Amazon SageMaker production variant that is dynamically scaled to use one to eight instances:   

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

### Defining a Target\-Tracking Scaling Policy<a name="endpoint-auto-scaling-add-code-define"></a>

To specify the metrics and target values for a scaling policy, you configure a target\-tracking scaling policy\. You can use either a predefined metric or a custom metric\.

Scaling policy configuration is represented by a JSON block\. You save your scaling policy configuration as a JSON block in a text file\. You use that text file when invoking the AWS CLI or the Application Auto Scaling API\. For more information about policy configuration syntax, see [http://docs.aws.amazon.com//autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html](http://docs.aws.amazon.com//autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the *Application Auto Scaling API Reference*\.

 The following options are available for defining a target\-tracking scaling policy configuration\.


+ [Using a Predefined Metric](#endpoint-auto-scaling-add-code-predefined)
+ [Using a Custom Metric](#endpoint-auto-scaling-add-code-custom)
+ [Adding a Cooldown Period](#endpoint-auto-scaling-add-code-cooldown)
+ [Disabling Scale\-in Activity](#endpoint-auto-scaling-add-code-scalein)

#### Using a Predefined Metric<a name="endpoint-auto-scaling-add-code-predefined"></a>

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

#### Using a Custom Metric<a name="endpoint-auto-scaling-add-code-custom"></a>

If you need to define a target\-tracking scaling policy that meets your custom requirements, define a custom metric\. You can define a custom metric based on any production variant metric that changes in proportion to scaling\. 

Not all Amazon SageMaker metrics work for target tracking\. The metric must be a valid utilization metric, and it must describe how busy an instance is\. The value of the metric must increase or decrease in inverse proportion to the number of variant instances\. That is, the value of the metric should decrease when the number of instances increases\.

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

#### Adding a Cooldown Period<a name="endpoint-auto-scaling-add-code-cooldown"></a>

To add a cooldown period for scaling out your variant, specify a value, in seconds, for `ScaleOutCooldown` \. Similarly, to add a cooldown period for scaling in your variant, add a value, in seconds, for `ScaleInCooldown` \. For more information about `ScaleInCooldown` and `ScaleOutCooldown`, see [http://docs.aws.amazon.com//autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html](http://docs.aws.amazon.com//autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the *Application Auto Scaling API Reference*\. 

**Example**  
The following is an example of a target\-tracking policy configuration for a scaling policy\. In this configuration, the `SageMakerVariantInvocationsPerInstance` predefined metric is used to adjust a variant based on an average of 70 across all instances of that variant\. The configuration provides a scale\-in cooldown period of 10 minutes and a scale\-out cooldown period of 5 minutes\.   

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

#### Disabling Scale\-in Activity<a name="endpoint-auto-scaling-add-code-scalein"></a>

You can prevent the target\-tracking scaling policy configuration from scaling in your variant by disabling scale\-in activity\. Disabling scale\-in activity prevents the scaling policy from deleting instances, while still allowing it to create them as needed\.

To enable or disable scale\-in activity for your variant, specify a Boolean value for `DisableScaleIn`\. For more information about `DisableScaleIn`, see [http://docs.aws.amazon.com//autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html](http://docs.aws.amazon.com//autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the *Application Auto Scaling API Reference*\. 

**Example**  
The following is an example of a target\-tracking configuration for a scaling policy\. In this configuration, the `SageMakerVariantInvocationsPerInstance` predefined metric adjusts a variant based on an average of 70 across all instances of that variant\. The configuration disables scale\-in activity for the scaling policy\.  

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

### Applying a Scaling Policy to a Production Variant<a name="endpoint-auto-scaling-add-code-apply"></a>

After registering your variant and defining a scaling policy, apply the scaling policy to the registered variant\. To apply a scaling policy to a variant, you can use the AWS CLI or the Application Auto Scaling API\. 

#### Applying a Scaling Policy \(AWS CLI\)<a name="endpoint-auto-scaling-add-code-apply-api"></a>

To apply a scaling policy to your variant, use the [http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) AWS CLI command with the following parameters:

+ `--policy-name`—The name of the scaling policy\.

+ `--policy-type`—Set this value to `TargetTrackingScaling`\.

+ `--resource-id`—The resource identifier for the variant\. For this parameter, the resource type is `endpoint` and the unique identifier is the name of the variant\. For example `endpoint/MyEndpoint/variant/MyVariant`\.

+ `--service-namespace`—Set this value to `sagemaker`\.

+ `--scalable-dimension`—Set this value to `sagemaker:variant:DesiredInstanceCount`\.

+ `--target-tracking-scaling-policy-configuration`—The target\-tracking scaling policy configuration to use for the variant\.

**Example**  
The following example uses with Application Auto Scaling to apply a target\-tracking scaling policy named `myscalablepolicy` to a variant named `myscalablevariant`\. The policy configuration is saved in a file named `config.json`\.  

```
aws application-autoscaling put-scaling-policy \
    --policy-name myscalablepolicy \
    --policy-type TargetTrackingScaling \
    --resource-id endpoint/MyEndpoint/variant/MyVariant \
    --service-namespace sagemaker \
    --scalable-dimension sagemaker:variant:DesiredInstanceCount \
    --target-tracking-scaling-policy-configuration file://config.json
```

#### Applying a Scaling Policy \(Application Auto Scaling API\)<a name="endpoint-auto-scaling-add-code-apply-api"></a>

To apply a scaling policy to a variant with the Application Auto Scaling API, use the [http://docs.aws.amazon.com//autoscaling/application/APIReference/API_PutScalingPolicy.html](http://docs.aws.amazon.com//autoscaling/application/APIReference/API_PutScalingPolicy.html) Application Auto Scaling API action with the following parameters:

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