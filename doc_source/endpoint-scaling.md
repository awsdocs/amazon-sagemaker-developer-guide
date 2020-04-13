# Update or delete endpoints that use automatic scaling<a name="endpoint-scaling"></a>

**Topics**
+ [Update endpoints that use automatic scaling](#endpoint-scaling-update)
+ [Delete endpoints configured for automatic scaling](#endpoint-delete-with-scaling)

## Update endpoints that use automatic scaling<a name="endpoint-scaling-update"></a>

When you update an endpoint, Application Auto Scaling checks to see whether any of the models on that endpoint are targets for automatic scaling\. If the update would change the instance type for any model that is a target for automatic scaling, the update fails\. 

In the AWS Management Console, you see a warning that you must deregister the model from automatic scaling before you can update it\. If you are trying to update the endpoint by calling the [ `UpdateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API, the call fails\. Before you update the endpoint, delete any scaling policies configured for it by calling the [DeleteScalingPolicy](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeleteScalingPolicy.html) Application Auto Scaling API action, then call [DeregisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeregisterScalableTarget.html) to deregister the variant as a scalable target\. After you update the endpoint, you can register the variant as a scalable target and attach an automatic scaling policy to the updated variant\.

There is one exception\. If you change the model for a variant that is configured for automatic scaling, Amazon SageMaker automatic scaling allows the update\. This is because changing the model doesn't typically affect performance enough to change automatic scaling behavior\. If you do update a model for a variant configured for automatic scaling, ensure that the change to the model doesn't significantly affect performance and automatic scaling behavior\.

When you update Amazon SageMaker endpoints that have automatic scaling applied, complete the following steps:

**To update an endpoint that has automatic scaling applied**

1. Deregister the endpoint as a scalable target by calling [DeregisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeregisterScalableTarget.html)\.

1. Because you turn off automatic scaling before you update the endpoint, you might want to take the additional precaution of increasing the number of instances for your endpoint during the update\. To do this, update the instance counts for the production variants hosted at the endpoint by calling [ `UpdateEndpointWeightsAndCapacities`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpointWeightsAndCapacities.html)\.

1. Call [ `DescribeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html) repeatedly until the value of the `EndpointStatus` field of the response is `InService`\.

1. Call [ `DescribeEndpointConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html) to get the values of the current endpoint config\.

1. Create a new endpoint config by calling [ `CreateEndpointConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)\. For the `InitialInstanceCount` field of each production variant, specify the corresponding value of `DesiredInstanceCount` from the response to the previous call to [ `DescribeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html)\. For all other values, use the values that you got as the response when you called [ `DescribeEndpointConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpointConfig.html) in the previous step\.

1. Update the endpoint by calling [ `UpdateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html)\. Specify the endpoint config you created in the previous step as the `EndpointConfig` field\.

1. Re\-enable automatic scaling by calling [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html)\.

## Delete endpoints configured for automatic scaling<a name="endpoint-delete-with-scaling"></a>

If you delete an endpoint, Application Auto Scaling checks to see whether any of the models on that endpoint are targets for automatic scaling\. If any are and you have permission to deregister the model, Application Auto Scaling deregisters those models as scalable targets without notifying you\. If you use a custom permission policy that doesn't provide permission for the [DeleteScalingPolicy](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeleteScalingPolicy.html) and [DeregisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeregisterScalableTarget.html) actions, you must delete automatic scaling policies and deregister scalable targets and before deleting the endpoint\.

**Note**  
You, as an IAM user, might not have sufficient permission to delete an endpoint if another IAM user configured automatic scaling for a variant on that endpoint\.