# Supported Instance Types and Availability Zones<a name="instance-types-az"></a>

This topic provides one table for each AWS Region supported by Amazon SageMaker\. Each table has a column for each Availability Zone in the Region\. For each Availability Zone, the Amazon SageMaker components that support each instance type are shown\. The components are listed in the tables as follows\.
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
+ [Component Support for Instances in US East \(Ohio\) us\-east\-2](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-us-east-ohio-us-east-2/)
+ [Component Support for Instances in US East \(N\. Virginia\) us\-east\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-n-virginia-us-east-1/)
+ [Component Support for Instances in US West \(N\. California\) us\-west\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-us-west-n-california-us-west-1/)
+ [Component Support for Instances in US West \(Oregon\) us\-west\-2](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-us-west-oregon-us-west-2/)
+ [Component Support for Instances in Asia Pacific \(Hong Kong\) ap\-east\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-hong-kong-ap-east-1/)
+ [Component Support for Instances in Asia Pacific \(Mumbai\) ap\-south\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-mumbai-ap-south-1/)
+ [Component Support for Instances in Asia Pacific \(Seoul\) ap\-northeast\-2](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-seoul-ap-northeast-2/)
+ [Component Support for Instances in Asia Pacific \(Singapore\) ap\-southeast\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-singapore-ap-southeast-1/)
+ [Component Support for Instances in Asia Pacific \(Sydney\) ap\-southeast\-2](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-sydney-ap-southeast-2/)
+ [Component Support for Instances in Asia Pacific \(Tokyo\) ap\-northeast\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-tokyo-ap-northeast-1/)
+ [Component Support for Instances in Canada \(Central\) ca\-central\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-canada-ca-central-1/)
+ [Component Support for Instances in EU \(Frankfurt\) eu\-central\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-frankfurt-eu-central-1/)
+ [Component Support for Instances in EU \(Ireland\) eu\-west\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-ireland-eu-west-1/)
+ [Component Support for Instances in EU \(London\) eu\-west\-2](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-london-eu-west-2/)
+ [Component Support for Instances in EU \(Paris\) eu\-west\-3](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-paris-eu-west-3/)
+ [Component Support for Instances in EU \(Stockholm\) eu\-north\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-stockholm-eu-north-1/)
+ [Component Support for Instances in Middle East \(Bahrain\) me\-south\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-bahrain-me-south-1/)
+ [Component Support for Instances in South America \(Sao Paulo\) sa\-east\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-sao-paulo-sa-east-1/)
+ [Component Support for Instances in AWS GovCloud \(US\-Gov\-West\) us\-gov\-west\-1](https://aws.amazon.com/releasenotes/sagemaker-instance-types-in-aws-govcloud-west-us-gov-west-1/)