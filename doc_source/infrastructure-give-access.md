# Give SageMaker Access to Resources in your Amazon VPC<a name="infrastructure-give-access"></a>

SageMaker runs the following job types in an Amazon Virtual Private Cloud by default\. 
+ Processing
+ Training
+ Model hosting
+ Batch transform
+ Amazon SageMaker Clarify
+ SageMaker Compilation

However, containers for these jobs access AWS resources—such as the Amazon Simple Storage Service \(Amazon S3\) buckets where you store training data and model artifacts—over the internet\.

To control access to your data and job containers, we recommend that you create a private VPC and configure it so that they aren't accessible over the internet\. For information about creating and configuring a VPC, see [Getting Started With Amazon VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/getting-started-ipv4.html) in the *Amazon VPC User Guide*\. Using a VPC helps to protect your job containers and data because you can configure your VPC so that it is not connected to the internet\. Using a VPC also allows you to monitor all network traffic in and out of your job containers by using VPC flow logs\. For more information, see [VPC Flow Logs](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html) in the *Amazon VPC User Guide*\.

You specify your private VPC configuration when you create jobs by specifying subnets and security groups\. When you specify the subnets and security groups, SageMaker creates *elastic network interfaces* that are associated with your security groups in one of the subnets\. Network interfaces allow your job containers to connect to resources in your VPC\. For information about network interfaces, see [Elastic Network Interfaces](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ElasticNetworkInterfaces.html) in the *Amazon VPC User Guide*\.

**Topics**
+ [Give SageMaker Processing Jobs Access to Resources in Your Amazon VPC](process-vpc.md)
+ [Give SageMaker Training Jobs Access to Resources in Your Amazon VPC](train-vpc.md)
+ [Give SageMaker Hosted Endpoints Access to Resources in Your Amazon VPC](host-vpc.md)
+ [Give Batch Transform Jobs Access to Resources in Your Amazon VPC](batch-vpc.md)
+ [Give Amazon SageMaker Clarify Jobs Access to Resources in Your Amazon VPC](clarify-vpc.md)
+ [Give SageMaker Compilation Jobs Access to Resources in Your Amazon VPC](neo-vpc.md)
+ [Give Inference Recommender Jobs Access to Resources in Your Amazon VPC](inference-recommender-vpc-access.md)