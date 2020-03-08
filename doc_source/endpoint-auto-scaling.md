# Automatically Scale Amazon SageMaker Models<a name="endpoint-auto-scaling"></a>

Amazon SageMaker supports automatic scaling for production variants\. *Automatic scaling* dynamically adjusts the number of instances provisioned for a production variant in response to changes in your workload\. When the workload increases, automatic scaling brings more instances online\. When the workload decreases, automatic scaling removes unnecessary instances so that you don't pay for provisioned variant instances that you aren't using\.

To use automatic scaling for a production variant, you define and apply a scaling policy that uses Amazon CloudWatch metrics and target values that you assign\. Automatic scaling uses the policy to increase or decrease the number of instances in response to actual workloads\.

You can use the AWS Management Console to apply a scaling policy based on a predefined metric\. A *predefined metric* is defined in an enumeration so that you can specify it by name in code or use it in the AWS Management Console\. Alternatively, you can use either the AWS Command Line Interface \(AWS CLI\) or the Application Auto Scaling API to apply a scaling policy based on a predefined or custom metric\. We strongly recommend that you load test your automatic scaling configuration to ensure that it works correctly before using it to manage production traffic\.

For information about deploying trained models as endpoints, see [Step 6\.1: Deploy the Model to Amazon SageMaker Hosting Services](ex1-deploy-model.md)\. 

**Topics**
+ [Automatic Scaling Components](#endpoint-auto-scaling-policy)
+ [Before You Begin](#endpoint-auto-scaling-target-byb)
+ [Related Topics](#w739aac31c37c17)
+ [Configure Automatic Scaling for a Production Variant](endpoint-auto-scaling-add-policy.md)
+ [Edit a Scaling Policy](endpoint-auto-scaling-edit.md)
+ [Delete a Scaling Policy](endpoint-auto-scaling-delete.md)
+ [Update Endpoints that Use Automatic Scaling](endpoint-scaling-update.md)
+ [Load Testing for Production Variant Automatic Scaling](endpoint-scaling-loadtest.md)
+ [Best Practices for Configuring Automatic Scaling](endpoint-auto-scaling-considerations.md)

## Automatic Scaling Components<a name="endpoint-auto-scaling-policy"></a>

To adjust the number of instances hosting a production variant, Amazon SageMaker automatic scaling uses a scaling policy \. Automatic scaling has the following components:
+ Required permissions—Permissions that are required to perform automatic scaling actions\.
+ A service\-linked role—An AWS Identity and Access Management \(IAM\) role that is linked to a specific AWS service\. A service\-linked role includes all of the permissions that the service requires to call other AWS services on your behalf\. Amazon SageMaker automatic scaling automatically generates this role, `AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint`, for you\.
+ A target metric—The Amazon CloudWatch metric that Amazon SageMaker automatic scaling uses to determine when and how much to scale\.
+ Minimum and maximum capacity—The minimum and maximum number of instances to use for scaling the variant\.
+ A cool down period—The amount of time, in seconds, after a scale\-in or scale\-out activity completes before another scale\-out activity can start\.

### Required Permissions for Automatic Scaling<a name="endpoint-auto-scaling-permissions"></a>

The `SagemakerFullAccessPolicy` IAM policy has all of the permissions required to perform automatic scaling actions\. For more information about Amazon SageMaker IAM roles, see [Amazon SageMaker Roles ](sagemaker-roles.md)\. 

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

### Service\-Linked Role for Automatic Scaling<a name="endpoint-auto-scaling-slr"></a>

A service\-linked role is a unique type of IAM role that is linked directly to an AWS service\. Service\-linked roles are predefined by the service and include all of the permissions that the service requires to call other AWS services on your behalf\. Automatic scaling uses the `AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint` service\-linked role\. For more information, see [Service\-Linked Roles for Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-service-linked-roles.html) in the *Application Auto Scaling User Guide*\.

### Target Metric for Automatic Scaling<a name="endpoint-auto-scaling-target-metric"></a>

Amazon SageMaker automatic scaling uses target\-tracking scaling policies\. You configure the *target\-tracking scaling policy* by specifying a predefined or custom metric and a target value for the metric \. For more information, see [Target Tracking Scaling Policies](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-target-tracking.html)\. 

Amazon CloudWatch alarms trigger the scaling policy , which calculate how to adjust scaling based on the metric and target value that you set\. The scaling policy adds or removes endpoint instances as required to keep the metric at, or close to, the specified target value\. In addition, a target\-tracking scaling policy also adjusts to fluctuations in the metric when a workload changes\. The policy minimizes rapid fluctuations in the number of available instances for your variant\.

For example, a scaling policy that uses the predefined `InvocationsPerInstance` metric with a target value of 70 can keep `InvocationsPerInstance` at, or close to 70\.

### Minimum and Maximum Capacity for Automatic Scaling<a name="endpoint-auto-scaling-target-capacity"></a>

You can specify the maximum number of endpoint instances that Application Auto Scaling manages for the variant\. The maximum value must be equal to or greater than the value specified for the minimum number of endpoint instances\. Amazon SageMaker automatic scaling does not enforce a limit for this value\.

You can also specify the minimum number of instances that Application Auto Scaling manages for the variant\. This value must be at least 1, and equal to or less than the value specified for the maximum number of variant instances\.

To determine the minimum and maximum number of instances that you need for typical traffic, test your automatic scaling configuration with the expected rate of traffic to your variant\.

### Cooldown Period for Automatic Scaling<a name="endpoint-auto-scaling-target-cooldown"></a>

Tune the responsiveness of a target\-tracking scaling policy by adding a cooldown period\. A *cooldown period* controls when your variant is scaled in and out by blocking subsequent scale\-in or scale\-out requests until the period expires\. This slows the deletion of variant instances for scale\-in requests, and the creation of variant instances for scale\-out requests\. A cooldown period helps to ensure that it doesn't launch or terminate additional instances before the previous scaling activity takes effect\. After automatic scaling dynamically scales using a scaling policy, it waits for the cooldown period to complete before resuming scaling activities\.

You configure the cooldown period in your automatic scaling policy\. You can specify the following cooldown periods:
+ A scale\-in activity reduces the number of variant instances\. A scale\-in cooldown period specifies the amount of time, in seconds, after a scale\-in activity completes before another scale\-in activity can start\. 
+ A scale\-out activity increases the number of variant instances\. A scale\-out cooldown period specifies the amount of time, in seconds, after a scale\-out activity completes before another scale\-out activity can start\. 

If you don't specify a scale\-in or a scale\-out cooldown period automatic scaling use the default, which is 300 seconds for each\.

 If instances are being added or removed too quickly when you test your automatic scaling configuration, consider increasing this value\. You can see this behavior if the traffic to your variant has a lot of spikes, or if you have multiple automatic scaling policies defined for a variant\.

If instances are not being added quickly enough to address increased traffic, consider decreasing this value\.

## Before You Begin<a name="endpoint-auto-scaling-target-byb"></a>

Before you can use automatically scaled model deployment, create an Amazon SageMaker model deployment\. For more information about deploying a model endpoint, see [Step 6\.1: Deploy the Model to Amazon SageMaker Hosting Services](ex1-deploy-model.md)\.

When automatic scaling adds a new variant instance, it is the same instance class as the one used by the primary instance\.

## Related Topics<a name="w739aac31c37c17"></a>
+ [What Is Application Auto Scaling?](https://docs.aws.amazon.com/autoscaling/application/userguide/what-is-application-auto-scaling.html)