# Supported Regions and Quotas<a name="regions-quotas"></a>

For a list of the AWS Regions supported by Amazon SageMaker and the Amazon Elastic Compute Cloud \(Amazon EC2\) instance types that are available in each Region, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

Each AWS Region is divided into sub\-regions known as Availability Zones\. For a given Region, the Availability Zones in that Region don't always contain the same instance types supported by SageMaker\. If any Availability Zone in the Region contains a given instance type, then that instance type is listed as available in the Region\.

For a list of the AWS Regions supported by Amazon SageMaker Studio, see [Amazon SageMaker Studio](studio.md)\.

For a list of the SageMaker service endpoints for each Region and the SageMaker service quotas for each instance type, see [Amazon SageMaker endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html)\.

You can request a quota increase using Service Quotas or the AWS Support Center\. To request an increase, see [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.

## Supported Instance Types and Availability Zones<a name="instance-types-az"></a>

This topic provides one table for each AWS Region supported by Amazon SageMaker\. Each table has a column for each Availability Zone in the Region\. For each Availability Zone, the SageMaker components that support each instance type are shown\. The components are listed in the tables as follows\.
+ **Notebook** – Notebook instances
+ **Training** – Training jobs
+ **Batch** – Batch transform jobs
+ **Endpoint** – Hosted endpoints

If no components support the instance type in an Availability Zone, the cell contains "None"\. If all components support the instance type in an Availability Zone, the cell contains "All"\.

Not all components are supported on each instance type in each Availability Zone\.

To create a component on a specific instance type, you must specify the Availability Zone ID, which is listed in the header row of each table, for example, `use1-az1`\.

**Note**  
Availability Zone names, for example, ` us-east-1a`, don't map directly to Availability Zone IDs\. For different AWS accounts, the same Availability Zone name might refer to a different Availability Zone ID\.

For more information, see [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide* and [AZ IDs for Your Resources](https://docs.aws.amazon.com/ram/latest/userguide/working-with-az-ids.html) in the *AWS RAM User Guide*\. 

**Region Tables**
+ [US East \(Ohio\) us\-east\-2](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-us-east-ohio-us-east-2/)
+ [US East \(N\. Virginia\) us\-east\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-n-virginia-us-east-1/)
+ [US West \(N\. California\) us\-west\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-us-west-n-california-us-west-1/)
+ [US West \(Oregon\) us\-west\-2](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-us-west-oregon-us-west-2/)
+ [Asia Pacific \(Hong Kong\) ap\-east\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-hong-kong-ap-east-1/)
+ [Asia Pacific \(Mumbai\) ap\-south\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-mumbai-ap-south-1/)
+ [Asia Pacific \(Seoul\) ap\-northeast\-2](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-seoul-ap-northeast-2/)
+ [Asia Pacific \(Singapore\) ap\-southeast\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-singapore-ap-southeast-1/)
+ [Asia Pacific \(Sydney\) ap\-southeast\-2](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-sydney-ap-southeast-2/)
+ [Asia Pacific \(Tokyo\) ap\-northeast\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-tokyo-ap-northeast-1/)
+ [Canada \(Central\) ca\-central\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-canada-ca-central-1/)
+ [EU \(Frankfurt\) eu\-central\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-frankfurt-eu-central-1/)
+ [EU \(Ireland\) eu\-west\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-ireland-eu-west-1/)
+ [EU \(London\) eu\-west\-2](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-london-eu-west-2/)
+ [EU \(Paris\) eu\-west\-3](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-paris-eu-west-3/)
+ [EU \(Stockholm\) eu\-north\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-stockholm-eu-north-1/)
+ [Middle East \(Bahrain\) me\-south\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-bahrain-me-south-1/)
+ [South America \(Sao Paulo\) sa\-east\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-sao-paulo-sa-east-1/)
+ [AWS GovCloud \(US\-Gov\-West\) us\-gov\-west\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-aws-govcloud-west-us-gov-west-1/)
+ [US ISO East us\-iso\-east\-1](http://aws.amazon.com/releasenotes/component-support-for-instances-in-us-iso-east/)