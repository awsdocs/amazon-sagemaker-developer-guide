# Best Practices for Configuring Automatic Scaling<a name="endpoint-auto-scaling-considerations"></a>

 When configuring automatic scaling, consider the following general guidelines\.

## Test Your Automatic Scaling Configuration<a name="endpoint-scaling-always-test"></a>

It is important that you test your automatic scaling configuration to confirm that it works with your model the way you expect it to\.

## Update Endpoints Configured for Automatic Scaling<a name="endpoint-update-with-scaling"></a>

When you update an endpoint, Application Auto Scaling checks to see whether any of the variants on that endpoint are targets for automatic scaling\. If the update would change the instance type for any variant that is a target for automatic scaling, the update fails\. 

In the AWS Management Console, you see a warning that you must deregister the variant from automatic scaling before you can update it\. If you are trying to update the endpoint by calling the [ `UpdateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API, the call fails\. Before you update the endpoint, delete any scaling policies configured for it by calling the [DeleteScalingPolicy](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeleteScalingPolicy.html) Application Auto Scaling API action, then call [DeregisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeregisterScalableTarget.html) to deregister the variant as a scalable target\. After you update the endpoint, you can register the variant as a scalable target and attach an automatic scaling policy to the updated variant\.

There is one exception\. If you change the model for a variant that is configured for automatic scaling, Amazon SageMaker automatic scaling allows the update\. This is because changing the model doesn't typically affect performance enough to change automatic scaling behavior\. If you do update a model for a variant configured for automatic scaling, ensure that the change to the model doesn't significantly affect performance and automatic scaling behavior\.

For instructions on how to update an endpoint that uses automatic scaling, see [Update Endpoints that Use Automatic Scaling](endpoint-scaling-update.md)\.

## Delete Endpoints Configured for Automatic Scaling<a name="endpoint-delete-with-scaling"></a>

If you delete an endpoint, Application Auto Scaling checks to see whether any of the variants on that endpoint are targets for automatic scaling\. If any are and you have permission to deregister the variant, Application Auto Scaling deregisters those variants as scalable targets without notifying you\. If you use a custom permission policy that doesn't provide permission for the [DeleteScalingPolicy](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeleteScalingPolicy.html) and [DeregisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeregisterScalableTarget.html) actions, you must delete automatic scaling policies and deregister scalable targets and before deleting the endpoint\.

**Note**  
You, as an IAM user, might not have sufficient permission to delete an endpoint if another IAM user configured automatic scaling for a variant on that endpoint\.

## Using Step Scaling Policies<a name="endpoint-scaling-step-policy"></a>

Although Amazon SageMaker automatic scaling supports using Application Auto Scaling step scaling policies, we recommend using target tracking policies, instead\. For information about using Application Auto Scaling step scaling policies, see [Step Scaling Policies](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-step-scaling-policies.html)\.

## Scaling In When There Is No Traffic<a name="endpoint-scaling-zero-traffic"></a>

If a variant’s traffic becomes zero, Amazon SageMaker automatic scaling doesn't scale in\. This is because Amazon SageMaker doesn't emit metrics with a value of zero\.

As a workaround, do either of the following:
+ Send requests to the variant until automatic scaling scales in to the minimum capacity
+ Change the policy to reduce the maximum provisioned capacity to match the minimum provisioned capacity