# Using Amazon SageMaker Ground Truth in an Amazon Virtual Private Cloud<a name="sms-vpc"></a>

[Amazon Virtual Private Cloud](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html) \(Amazon VPC\) is a service with which you can launch AWS resources in a logically isolated virtual network that you define\. You can create and run a Ground Truth labeling job inside of an Amazon VPC instead of connecting over the internet\. When you launch a labeling job in an Amazon VPC, communication between your VPC and Ground Truth is conducted entirely and securely within the AWS network\.

This guide shows how you can use Ground Truth in an Amazon VPC in the following ways:

1. [Run an Amazon SageMaker Ground Truth Labeling Job in an Amazon Virtual Private Cloud](samurai-vpc-labeling-job.md)

1. [Use Amazon VPC Mode from a Private Worker Portal](samurai-vpc-worker-portal.md)