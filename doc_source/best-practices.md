# Deployment Best Practices<a name="best-practices"></a>

This topic provides guidance on best practices for deploying machine learning models in Amazon SageMaker\.

## Deploy Multiple Instances Across Avalibility Zones<a name="deployment-best-practices"></a>

**Create robust endpoints when hosting your model\.** SageMaker endpoints can help protect your application from [Availability Zone](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) outages and instance failures\. If an outage occurs or an instance fails, SageMaker automatically attempts to distribute your instances across Availability Zones\. For this reason, we strongly recommended that you deploy multiple instances for each production endpoint\. 

If you are using an [Amazon Virtual Private Cloud \(VPC\)](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html), configure the VPC with at least two [ `Subnets`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_VpcConfig.html#SageMaker-Type-VpcConfig-Subnets                 .html), each in a different Availability Zone\. If an outage occurs or an instance fails, Amazon SageMaker automatically attempts to distribute your instances across Availability Zones\. 

In general, to achieve more reliable performance, use more small [Instance Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in different Availability Zones to host your endpoints\.