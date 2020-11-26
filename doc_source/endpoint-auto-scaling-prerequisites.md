# Prerequisites<a name="endpoint-auto-scaling-prerequisites"></a>

Before you can use autoscaling, must have already created a Amazon SageMaker model deployment\. Deployed models are referred to as a [production variant](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html)\. This includes information about the model and the resources used to host it\. 

For more information about deploying a model endpoint, see [Step 6\.1: Deploy the Model to SageMaker Hosting Services](ex1-deploy-model.md)\.

To enable autoscaling for a model, you can use the console, the AWS CLI, or the Application Auto Scaling API\. It is recommended to try to [Configure model autoscaling with the console](endpoint-auto-scaling-add-console.md) to get familiar with the requirements and to test your first autoscaling configuration\. When using the AWS CLI or Application Auto Scaling the flow is to register the model, define the scaling policy, then apply it\. The following overview provides further details on the prerequisites and components used with autoscaling\.

## Autoscaling policy overview<a name="endpoint-auto-scaling-policy"></a>

To use automatic scaling, you define and apply a scaling policy that uses Amazon CloudWatch metrics and target values that you assign\. Automatic scaling uses the policy to increase or decrease the number of instances in response to actual workloads\.

You can use the AWS Management Console to apply a scaling policy based on a predefined metric\. A *predefined metric* is defined in an enumeration so that you can specify it by name in code or use it in the AWS Management Console\. Alternatively, you can use either the AWS Command Line Interface \(AWS CLI\) or the Application Auto Scaling API to apply a scaling policy based on a predefined or custom metric\. 

There are two types of supported scaling policies: target\-tracking scaling and step scaling\. It is recommended to use target\-tracking scaling policies for your autoscaling configuration\. You configure the *target\-tracking scaling policy* by specifying a predefined or custom metric and a target value for the metric\. For more information about using Application Auto Scaling target\-tracking scaling policies, see [ Target Tracking Scaling Policies](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-target-tracking.html)\.

You can use step scaling when you require an advanced configuration, such as specifying how many instances to deploy under what conditions\. Otherwise, using target\-tracking scaling is preferred as it will be fully automated\. For more information about using Application Auto Scaling step scaling policies, see [ Step Scaling Policies](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-step-scaling-policies.html)\.

A scaling policy has the following components:
+ A target metric—The Amazon CloudWatch metric that SageMaker automatic scaling uses to determine when and how much to scale\.
+ Minimum and maximum capacity—The minimum and maximum number of instances to use for scaling\.
+ A cool down period—The amount of time, in seconds, after a scale\-in or scale\-out activity completes before another scale\-out activity can start\.
+ Required permissions—Permissions that are required to perform automatic scaling actions\.
+ A service\-linked role—An AWS Identity and Access Management \(IAM\) role that is linked to a specific AWS service\. A service\-linked role includes all of the permissions that the service requires to call other AWS services on your behalf\. SageMaker automatic scaling automatically generates this role, `AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint`, for you\.

## Target metric for autoscaling<a name="endpoint-auto-scaling-target-metric"></a>

Amazon CloudWatch alarms trigger the scaling policy, which calculate how to adjust scaling based on the metric and target value that you set\. The scaling policy adds or removes endpoint instances as required to keep the metric at, or close to, the specified target value\. In addition, a scaling policy also adjusts to fluctuations in the metric when a workload changes\. The scaling policy minimizes rapid fluctuations in the number of available instances for your model\.

For example, a scaling policy that uses the predefined `InvocationsPerInstance` metric with a target value of 70 can keep `InvocationsPerInstance` at, or close to 70\.

## Minimum and maximum capacity<a name="endpoint-auto-scaling-target-capacity"></a>

You may specify the maximum number of endpoint instances for the model\. The maximum value must be equal to or greater than the value specified for the minimum number of endpoint instances\. SageMaker automatic scaling does not enforce a limit for this value\.

You must also specify the minimum number of instances for the model\. This value must be at least 1, and equal to or less than the value specified for the maximum number of endpoint instances\.

To determine the minimum and maximum number of instances that you need for typical traffic, test your autoscaling configuration with the expected rate of traffic to your model\.

**Important**  
Scaling\-in does not occur when there is no traffic: if a variant’s traffic becomes zero, SageMaker automatic scaling doesn't scale in\. This is because SageMaker doesn't emit metrics with a value of zero\. 

## Cooldown period<a name="endpoint-auto-scaling-target-cooldown"></a>

Tune the responsiveness of your scaling policy by adding a cooldown period\. A *cooldown period* controls when your model is scaled\-in \(by reducing instances\) or scaled\-out \(by increasing instances\)\. It does this by blocking subsequent scale\-in or scale\-out requests until the period expires\. This slows the deletion of instances for scale\-in requests, and the creation of instances for scale\-out requests\. A cooldown period helps to ensure that the scaling policy doesn't launch or terminate additional instances before the previous scaling activity takes effect\. After automatic scaling dynamically scales using a scaling policy, it waits for the cooldown period to complete before resuming scaling activities\.

You configure the cooldown period in your automatic scaling policy\. You can specify the following cooldown periods:
+ A scale\-in activity reduces the number of instances\. A scale\-in cooldown period specifies the amount of time, in seconds, after a scale\-in activity completes before another scale\-in activity can start\. 
+ A scale\-out activity increases the number of instances\. A scale\-out cooldown period specifies the amount of time, in seconds, after a scale\-out activity completes before another scale\-out activity can start\. 

If you don't specify a scale\-in or a scale\-out cooldown period automatic scaling use the default, which is 300 seconds for each\.

If instances are being added or removed too quickly when you test your automatic scaling configuration, consider increasing this value\. You can see this behavior if the traffic to your model has a lot of spikes, or if you have multiple automatic scaling policies defined for a variant\.

If instances are not being added quickly enough to address increased traffic, consider decreasing this value\.

## Permissions<a name="endpoint-auto-scaling-permissions"></a>

The `SagemakerFullAccessPolicy` IAM policy has all of the IAM permissions required to perform autoscaling\. For more information about SageMaker IAM permissions, see [SageMaker Roles ](sagemaker-roles.md)\.

If you are using a custom permission policy, you must include the following permissions:

```
{
  "Effect": "Allow",
  "Action": [
    "sagemaker:DescribeEndpoint",
    "sagemaker:DescribeEndpointConfig",
    "sagemaker:UpdateEndpointWeightsAndCapacities"
  ],
  "Resource": "*"
}
                {
    "Action": [
        "application-autoscaling:*"
    ],
    "Effect": "Allow",
    "Resource": "*"
}

{
  "Action": "iam:CreateServiceLinkedRole",
  "Effect": "Allow",
  "Resource":
  "arn:aws:iam::*:role/aws-service-role/sagemaker.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint",
  "Condition": {
    "StringLike": { "iam:AWSServiceName": "sagemaker.application-autoscaling.amazonaws.com"	}
  }
}

{
  "Effect": "Allow",
  "Action": [
    "cloudwatch:PutMetricAlarm",
    "cloudwatch:DescribeAlarms",
    "cloudwatch:DeleteAlarms"
  ],
  "Resource": "*"
}
```

## Service\-linked role<a name="endpoint-auto-scaling-slr"></a>

Autoscaling uses the `AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint` service\-linked role; it created for you automatically\. A service\-linked role is a unique type of IAM role that is linked directly to an AWS service\. Service\-linked roles are predefined by the service and include all of the permissions that the service requires to call other AWS services on your behalf\. For more information, see [Service\-Linked Roles](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-service-linked-roles.html)\.