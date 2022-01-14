# Real\-time Inference<a name="realtime-endpoints"></a>

Real\-time inference is ideal for inference workloads where you have real\-time, interactive, low latency requirements\. You can deploy your model to SageMaker hosting services and get an endpoint that can be used for inference\. These endpoints are fully managed, support autoscaling \(see [Automatically Scale Amazon SageMaker Models](endpoint-auto-scaling.md)\), and can be deployed in multiple [Availability Zones](instance-types-az.md)\.

You can create a real\-time inference endpoint using the AWS SDK for Python \(Boto3\) or the AWS CLI\.

**Topics**
+ [Create Your Endpoint and Deploy Your Model](realtime-endpoints-deployment.md)
+ [Test Endpoints](realtime-endpoints-test-endpoints.md)
+ [Delete Endpoints and Resources](realtime-endpoints-delete-resources.md)
+ [Best Practices for Deploying Models on SageMaker Hosting Services](how-it-works-hosting-related-considerations.md)