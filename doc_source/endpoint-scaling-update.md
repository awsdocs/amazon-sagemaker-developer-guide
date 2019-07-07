# Update Endpoints that Use Automatic Scaling<a name="endpoint-scaling-update"></a>

When you update Amazon SageMaker endpoints that have automatic scaling applied, complete the following steps:

**To update an endpoint that has automatic scaling applied**

1. Deregister the endpoint as a scalable target by calling [DeregisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeregisterScalableTarget.html)\.

1. Because you turn off automatic scaling before you update the endpoint, you might want to take the additional precaution of increasing the number of instances for your endpoint during the update\. To do this, update the instance counts for the production variants hosted at the endpoint by calling [UpdateEndpointWeightsAndCapacities](API_UpdateEndpointWeightsAndCapacities.md)\.

1. Call [DescribeEndpoint](API_DescribeEndpoint.md) repeatedly until the value of the `EndpointStatus` field of the response is `InService`\.

1. Call [DescribeEndpointConfig](API_DescribeEndpointConfig.md) to get the values of the current endpoint config\.

1. Create a new endpoint config by calling [CreateEndpointConfig](API_CreateEndpointConfig.md)\. For the `InitialInstanceCount` field of each production variant, specify the corresponding value of `DesiredInstanceCount` from the response to the previous call to [DescribeEndpoint](API_DescribeEndpoint.md)\. For all other values, use the values that you got as the response when you called [DescribeEndpointConfig](API_DescribeEndpointConfig.md) in the previous step\.

1. Update the endpoint by calling [UpdateEndpoint](API_UpdateEndpoint.md)\. Specify the endpoint config you created in the previous step as the `EndpointConfig` field\.

1. Re\-enable automatic scaling by calling [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html)\.