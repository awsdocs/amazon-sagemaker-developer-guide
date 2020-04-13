# Supported Instance Types and Availability Zones<a name="instance-types-az"></a>

The tables in this topic list the availability of instance types for each AWS Region and Availability Zone for the following Amazon SageMaker components\.
+ Notebook instances – listed as `Notebook` in the tables
+ Training jobs – listed as `Training` in the tables
+ Batch transform jobs – listed as `Batch` in the tables
+ Hosted endpoints – listed as `Endpoint` in the tables

Not all components are supported on each instance type in each Availability Zone\.

There is one table for each AWS Region\. In each table, the Amazon SageMaker components that support each instance type are listed for each Availability Zone\. If no components support the instance type in an Availability Zone, the cell contains "None"\. If all components support the instance type in an Availability Zone, the cell contains "All"\.

To create a component on a specific instance type, you must specify the Availability Zone ID, which is listed in the header row of each table, for example, `use1-az1`\. Availability Zone names, for example, ` us-east-1a`, don't map directly to Availability Zone IDs\. For different AWS accounts, the same Availability Zone name might refer to a different Availability Zone ID\.

For more information, see [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide* and [AZ IDs for Your Resources](https://docs.aws.amazon.com/ram/latest/userguide/working-with-az-ids.html) in the *AWS RAM User Guide*\. 

**Topics**
+ [Component Support for Instances in US East \(Ohio\) us\-east\-2](#us-east-2-instances)
+ [Component Support for Instances in US East \(N\. Virginia\) us\-east\-1](#us-east-1-instances)
+ [Component Support for Instances in US West \(N\. California\) us\-west\-1](#us-west-1-instances)
+ [Component Support for Instances in US West \(Oregon\) us\-west\-2](#us-west-2-instances)
+ [Component Support for Instances in Asia Pacific \(Hong Kong\) ap\-east\-1](#ap-east-1-instances)
+ [Component Support for Instances in Asia Pacific \(Mumbai\) ap\-south\-1](#ap-south-1-instances)
+ [Component Support for Instances in Asia Pacific \(Seoul\) ap\-northeast\-2](#ap-northeast-2-instances)
+ [Component Support for Instances in Asia Pacific \(Singapore\) ap\-southeast\-1](#ap-southeast-1-instances)
+ [Component Support for Instances in Asia Pacific \(Sydney\) ap\-southeast\-2](#ap-southeast-2-instances)
+ [Component Support for Instances in Asia Pacific \(Tokyo\) ap\-northeast\-1](#ap-northeast-1-instances)
+ [Component Support for Instances in Canada \(Central\) ca\-central\-1](#ca-central-1-instances)
+ [Component Support for Instances in EU \(Frankfurt\) eu\-central\-1](#eu-central-1-instances)
+ [Component Support for Instances in EU \(Ireland\) eu\-west\-1](#eu-west-1-instances)
+ [Component Support for Instances in EU \(London\) eu\-west\-2](#eu-west-2-instances)
+ [Component Support for Instances in EU \(Paris\) eu\-west\-3](#eu-west-3-instances)
+ [Component Support for Instances in EU \(Stockholm\) eu\-north\-1](#eu-north-1-instances)
+ [Component Support for Instances in Middle East \(Bahrain\) me\-south\-1](#me-south-1-instances)
+ [Component Support for Instances in South America \(Sao Paulo\) sa\-east\-1](#sa-east-1-instances)
+ [Component Support for Instances in AWS GovCloud \(US\-Gov\-West\) us\-gov\-west\-1](#us-gov-west-1-instances)

## Component Support for Instances in US East \(Ohio\) us\-east\-2<a name="us-east-2-instances"></a>


| Instance Type | use2\-az1 | use2\-az2 | use2\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.m4\.xlarge | All | All | All | 
| ml\.m4\.2xlarge | All | All | All | 
| ml\.m4\.4xlarge | All | All | All | 
| ml\.m4\.10xlarge | All | All | All | 
| ml\.m4\.16xlarge | Notebook, Training, Batch | All | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c4\.xlarge | All | All | All | 
| ml\.c4\.2xlarge | All | All | All | 
| ml\.c4\.4xlarge | All | All | All | 
| ml\.c4\.8xlarge | All | All | All | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | All | 
| ml\.c5\.2xlarge | All | All | All | 
| ml\.c5\.4xlarge | All | All | All | 
| ml\.c5\.9xlarge | All | All | All | 
| ml\.c5\.18xlarge | All | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | Notebook | 
| ml\.p2\.xlarge | All | All | All | 
| ml\.p2\.8xlarge | All | All | All | 
| ml\.p2\.16xlarge | Notebook, Training, Batch | All | Notebook, Training, Batch | 
| ml\.p3\.2xlarge | All | All | None | 
| ml\.p3\.8xlarge | All | All | None | 
| ml\.p3\.16xlarge | All | All | None | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia1\.large | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia1\.xlarge | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia2\.medium | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia2\.large | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | None | 

## Component Support for Instances in US East \(N\. Virginia\) us\-east\-1<a name="us-east-1-instances"></a>


| Instance Type | use1\-az1 | use1\-az2 | use1\-az3 | use1\-az4 | use1\-az5 | use1\-az6 | 
| --- | --- | --- | --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | None | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.large | Notebook | Notebook | None | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.xlarge | Notebook | Notebook | None | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.2xlarge | Notebook | Notebook | None | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.m4\.xlarge | All | All | All | All | All | All | 
| ml\.m4\.2xlarge | All | All | All | All | All | All | 
| ml\.m4\.4xlarge | All | All | All | All | All | All | 
| ml\.m4\.10xlarge | All | All | All | All | All | All | 
| ml\.m4\.16xlarge | All | All | All | All | All | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch | None | Training, Batch, Endpoint | Training, Batch | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | None | All | All | All | 
| ml\.m5\.2xlarge | All | All | None | All | All | All | 
| ml\.m5\.4xlarge | All | All | None | All | All | All | 
| ml\.m5\.12xlarge | All | All | None | All | All | All | 
| ml\.m5\.24xlarge | All | All | None | All | All | All | 
| ml\.r5\.large | None | None | None | None | None | None | 
| ml\.r5\.xlarge | None | None | None | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | Endpoint | Endpoint | Endpoint | Endpoint | 
| ml\.c4\.xlarge | All | All | None | All | All | All | 
| ml\.c4\.2xlarge | All | All | None | All | All | All | 
| ml\.c4\.4xlarge | All | All | None | All | All | All | 
| ml\.c4\.8xlarge | All | All | None | All | Notebook, Endpoint | All | 
| ml\.c5\.large | Endpoint | Endpoint | None | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | Batch | All | All | All | 
| ml\.c5\.2xlarge | All | All | None | All | All | All | 
| ml\.c5\.4xlarge | All | All | None | All | All | All | 
| ml\.c5\.9xlarge | All | All | None | All | Notebook, Batch, Endpoint | All | 
| ml\.c5\.18xlarge | All | All | None | All | All | All | 
| ml\.c5d\.xlarge | Notebook, Endpoint | Notebook, Endpoint | None | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | None | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.4xlarge | Notebook, Endpoint | Notebook, Endpoint | None | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.9xlarge | Notebook, Endpoint | Notebook, Endpoint | None | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.18xlarge | Notebook, Endpoint | Notebook, Endpoint | None | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.p2\.xlarge | All | All | Notebook, Training, Batch | All | Notebook, Endpoint | All | 
| ml\.p2\.8xlarge | Notebook, Training, Batch | All | Notebook, Training, Batch | All | None | All | 
| ml\.p2\.16xlarge | Notebook, Training, Batch | Notebook, Training, Batch | Notebook, Training, Batch | All | None | Notebook, Training, Batch | 
| ml\.p3\.2xlarge | All | Training | None | None | All | Notebook, Training, Batch | 
| ml\.p3\.8xlarge | All | Training | None | None | All | All | 
| ml\.p3\.16xlarge | All | Training | None | None | All | All | 
| ml\.p3dn\.24xlarge | Training | None | None | None | None | Training | 
| ml\.g4dn\.xlarge | None | None | None | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | None | None | None | 
| ml\.eia1\.medium | Notebook, Endpoint | Notebook, Endpoint | None | None | None | Notebook, Endpoint | 
| ml\.eia1\.large | Notebook, Endpoint | Notebook, Endpoint | None | None | None | Notebook, Endpoint | 
| ml\.eia1\.xlarge | Notebook, Endpoint | Notebook, Endpoint | None | None | None | Notebook, Endpoint | 
| ml\.eia2\.medium | Notebook, Endpoint | Notebook, Endpoint | None | None | None | Notebook, Endpoint | 
| ml\.eia2\.large | Notebook, Endpoint | Notebook, Endpoint | None | None | None | Notebook, Endpoint | 
| ml\.eia2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | None | None | None | Notebook, Endpoint | 

## Component Support for Instances in US West \(N\. California\) us\-west\-1<a name="us-west-1-instances"></a>


| Instance Type | usw1\-az1 | usw1\-az3 | 
| --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | 
| ml\.m4\.xlarge | All | All | 
| ml\.m4\.2xlarge | All | All | 
| ml\.m4\.4xlarge | All | All | 
| ml\.m4\.10xlarge | All | All | 
| ml\.m4\.16xlarge | All | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | 
| ml\.m5\.2xlarge | All | All | 
| ml\.m5\.4xlarge | All | All | 
| ml\.m5\.12xlarge | All | All | 
| ml\.m5\.24xlarge | All | All | 
| ml\.r5\.large | None | None | 
| ml\.r5\.xlarge | None | None | 
| ml\.r5\.2xlarge | None | None | 
| ml\.r5\.4xlarge | None | None | 
| ml\.r5\.12xlarge | None | None | 
| ml\.r5\.24xlarge | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | 
| ml\.c4\.xlarge | All | All | 
| ml\.c4\.2xlarge | All | All | 
| ml\.c4\.4xlarge | All | All | 
| ml\.c4\.8xlarge | All | All | 
| ml\.c5\.large | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | 
| ml\.c5\.2xlarge | All | All | 
| ml\.c5\.4xlarge | All | All | 
| ml\.c5\.9xlarge | All | All | 
| ml\.c5\.18xlarge | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | 
| ml\.p2\.xlarge | None | None | 
| ml\.p2\.8xlarge | None | None | 
| ml\.p2\.16xlarge | None | None | 
| ml\.p3\.2xlarge | None | None | 
| ml\.p3\.8xlarge | None | None | 
| ml\.p3\.16xlarge | None | None | 
| ml\.p3dn\.24xlarge | None | None | 
| ml\.g4dn\.xlarge | None | None | 
| ml\.g4dn\.2xlarge | None | None | 
| ml\.g4dn\.4xlarge | None | None | 
| ml\.g4dn\.8xlarge | None | None | 
| ml\.g4dn\.12xlarge | None | None | 
| ml\.g4dn\.16xlarge | None | None | 
| ml\.eia1\.medium | None | None | 
| ml\.eia1\.large | None | None | 
| ml\.eia1\.xlarge | None | None | 
| ml\.eia2\.medium | None | None | 
| ml\.eia2\.large | None | None | 
| ml\.eia2\.xlarge | None | None | 

## Component Support for Instances in US West \(Oregon\) us\-west\-2<a name="us-west-2-instances"></a>


| Instance Type | usw2\-az1 | usw2\-az2 | usw2\-az3 | usw2\-az4 | 
| --- | --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.t3\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.t3\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.t3\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.t3\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.m4\.xlarge | All | All | All | None | 
| ml\.m4\.2xlarge | All | All | All | None | 
| ml\.m4\.4xlarge | All | All | All | None | 
| ml\.m4\.10xlarge | All | All | All | None | 
| ml\.m4\.16xlarge | All | All | All | None | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | None | 
| ml\.m5\.xlarge | All | All | All | None | 
| ml\.m5\.2xlarge | All | All | All | None | 
| ml\.m5\.4xlarge | All | All | All | None | 
| ml\.m5\.12xlarge | All | All | All | None | 
| ml\.m5\.24xlarge | All | All | All | None | 
| ml\.r5\.large | None | None | None | None | 
| ml\.r5\.xlarge | None | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | Endpoint | None | 
| ml\.c4\.xlarge | All | All | All | None | 
| ml\.c4\.2xlarge | All | All | All | None | 
| ml\.c4\.4xlarge | All | All | All | None | 
| ml\.c4\.8xlarge | All | All | All | None | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | None | 
| ml\.c5\.xlarge | All | All | All | None | 
| ml\.c5\.2xlarge | All | All | All | None | 
| ml\.c5\.4xlarge | All | All | All | None | 
| ml\.c5\.9xlarge | All | All | All | None | 
| ml\.c5\.18xlarge | All | All | All | None | 
| ml\.c5d\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.c5d\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.c5d\.4xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.c5d\.9xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.c5d\.18xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.p2\.xlarge | All | All | All | None | 
| ml\.p2\.8xlarge | Notebook, Training, Batch | Notebook, Training, Batch | Notebook, Training, Batch | None | 
| ml\.p2\.16xlarge | Notebook, Training, Batch | Notebook, Training, Batch | Notebook, Training, Batch | None | 
| ml\.p3\.2xlarge | All | Notebook, Training, Endpoint | All | None | 
| ml\.p3\.8xlarge | All | Notebook, Training, Endpoint | All | None | 
| ml\.p3\.16xlarge | All | Notebook, Training, Endpoint | All | None | 
| ml\.p3dn\.24xlarge | Training | None | Training | None | 
| ml\.g4dn\.xlarge | None | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | None | 
| ml\.eia1\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia1\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia1\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 
| ml\.eia2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | None | 

## Component Support for Instances in Asia Pacific \(Hong Kong\) ap\-east\-1<a name="ap-east-1-instances"></a>


| Instance Type | ape1\-az1 | ape1\-az2 | ape1\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | None | None | None | 
| ml\.t2\.large | None | None | None | 
| ml\.t2\.xlarge | None | None | None | 
| ml\.t2\.2xlarge | None | None | None | 
| ml\.t3\.medium | Notebook | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.m4\.xlarge | None | None | None | 
| ml\.m4\.2xlarge | None | None | None | 
| ml\.m4\.4xlarge | None | None | None | 
| ml\.m4\.10xlarge | None | None | None | 
| ml\.m4\.16xlarge | None | None | None | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | None | None | None | 
| ml\.c4\.xlarge | None | None | None | 
| ml\.c4\.2xlarge | None | None | None | 
| ml\.c4\.4xlarge | None | None | None | 
| ml\.c4\.8xlarge | None | None | None | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | All | 
| ml\.c5\.2xlarge | All | All | All | 
| ml\.c5\.4xlarge | All | All | All | 
| ml\.c5\.9xlarge | All | All | All | 
| ml\.c5\.18xlarge | All | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | Notebook | 
| ml\.p2\.xlarge | None | None | None | 
| ml\.p2\.8xlarge | None | None | None | 
| ml\.p2\.16xlarge | None | None | None | 
| ml\.p3\.2xlarge | None | None | None | 
| ml\.p3\.8xlarge | None | None | None | 
| ml\.p3\.16xlarge | None | None | None | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in Asia Pacific \(Mumbai\) ap\-south\-1<a name="ap-south-1-instances"></a>


| Instance Type | aps1\-az1 | aps1\-az2 | aps1\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t3\.medium | None | None | None | 
| ml\.t3\.large | None | None | None | 
| ml\.t3\.xlarge | None | None | None | 
| ml\.t3\.2xlarge | None | None | None | 
| ml\.m4\.xlarge | All | None | All | 
| ml\.m4\.2xlarge | All | None | All | 
| ml\.m4\.4xlarge | All | None | All | 
| ml\.m4\.10xlarge | All | None | All | 
| ml\.m4\.16xlarge | All | None | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Batch | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | Batch | All | 
| ml\.m5\.2xlarge | All | Batch | All | 
| ml\.m5\.4xlarge | All | Batch | All | 
| ml\.m5\.12xlarge | All | Batch | All | 
| ml\.m5\.24xlarge | All | Batch | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | None | Endpoint | 
| ml\.c4\.xlarge | All | Batch | All | 
| ml\.c4\.2xlarge | All | Batch | All | 
| ml\.c4\.4xlarge | All | Batch | All | 
| ml\.c4\.8xlarge | All | Batch | All | 
| ml\.c5\.large | Endpoint | None | Endpoint | 
| ml\.c5\.xlarge | All | None | All | 
| ml\.c5\.2xlarge | All | None | All | 
| ml\.c5\.4xlarge | All | None | All | 
| ml\.c5\.9xlarge | All | None | All | 
| ml\.c5\.18xlarge | All | None | All | 
| ml\.c5d\.xlarge | None | None | None | 
| ml\.c5d\.2xlarge | None | None | None | 
| ml\.c5d\.4xlarge | None | None | None | 
| ml\.c5d\.9xlarge | None | None | None | 
| ml\.c5d\.18xlarge | None | None | None | 
| ml\.p2\.xlarge | All | None | All | 
| ml\.p2\.8xlarge | All | None | All | 
| ml\.p2\.16xlarge | All | None | All | 
| ml\.p3\.2xlarge | None | None | None | 
| ml\.p3\.8xlarge | None | None | None | 
| ml\.p3\.16xlarge | None | None | None | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in Asia Pacific \(Seoul\) ap\-northeast\-2<a name="ap-northeast-2-instances"></a>


| Instance Type | apne2\-az1 | apne2\-az2 | apne2\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t3\.medium | None | None | None | 
| ml\.t3\.large | None | None | None | 
| ml\.t3\.xlarge | None | None | None | 
| ml\.t3\.2xlarge | None | None | None | 
| ml\.m4\.xlarge | All | None | All | 
| ml\.m4\.2xlarge | All | None | All | 
| ml\.m4\.4xlarge | All | None | All | 
| ml\.m4\.10xlarge | All | None | All | 
| ml\.m4\.16xlarge | All | None | All | 
| ml\.m5\.large | Training, Batch, Endpoint | None | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | None | All | 
| ml\.m5\.2xlarge | All | None | All | 
| ml\.m5\.4xlarge | All | None | All | 
| ml\.m5\.12xlarge | All | None | All | 
| ml\.m5\.24xlarge | All | None | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | None | Endpoint | 
| ml\.c4\.xlarge | All | None | All | 
| ml\.c4\.2xlarge | All | None | All | 
| ml\.c4\.4xlarge | All | None | All | 
| ml\.c4\.8xlarge | All | None | All | 
| ml\.c5\.large | Endpoint | None | Endpoint | 
| ml\.c5\.xlarge | All | None | All | 
| ml\.c5\.2xlarge | All | None | All | 
| ml\.c5\.4xlarge | All | None | All | 
| ml\.c5\.9xlarge | All | None | All | 
| ml\.c5\.18xlarge | All | None | All | 
| ml\.c5d\.xlarge | Notebook | None | Notebook | 
| ml\.c5d\.2xlarge | Notebook | None | Notebook | 
| ml\.c5d\.4xlarge | Notebook | None | Notebook | 
| ml\.c5d\.9xlarge | Notebook | None | Notebook | 
| ml\.c5d\.18xlarge | Notebook | None | Notebook | 
| ml\.p2\.xlarge | All | None | All | 
| ml\.p2\.8xlarge | All | None | All | 
| ml\.p2\.16xlarge | All | None | All | 
| ml\.p3\.2xlarge | All | None | All | 
| ml\.p3\.8xlarge | All | None | All | 
| ml\.p3\.16xlarge | All | None | All | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.eia1\.large | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.eia1\.xlarge | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.eia2\.medium | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.eia2\.large | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.eia2\.xlarge | Notebook, Endpoint | None | Notebook, Endpoint | 

## Component Support for Instances in Asia Pacific \(Singapore\) ap\-southeast\-1<a name="ap-southeast-1-instances"></a>


| Instance Type | apse1\-az1 | apse1\-az2 | apse1\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.m4\.xlarge | All | All | All | 
| ml\.m4\.2xlarge | All | All | All | 
| ml\.m4\.4xlarge | All | All | Notebook, Endpoint | 
| ml\.m4\.10xlarge | All | All | Notebook, Endpoint | 
| ml\.m4\.16xlarge | All | All | Notebook, Endpoint | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c4\.xlarge | All | All | All | 
| ml\.c4\.2xlarge | All | All | All | 
| ml\.c4\.4xlarge | All | All | All | 
| ml\.c4\.8xlarge | All | All | All | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | All | 
| ml\.c5\.2xlarge | All | All | All | 
| ml\.c5\.4xlarge | All | All | All | 
| ml\.c5\.9xlarge | All | All | All | 
| ml\.c5\.18xlarge | All | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | Notebook | 
| ml\.p2\.xlarge | All | All | None | 
| ml\.p2\.8xlarge | All | All | None | 
| ml\.p2\.16xlarge | All | All | None | 
| ml\.p3\.2xlarge | Notebook, Training, Batch | Notebook, Training, Batch | None | 
| ml\.p3\.8xlarge | Notebook, Training, Batch | Notebook, Training, Batch | None | 
| ml\.p3\.16xlarge | Notebook, Training, Batch | Notebook, Training, Batch | None | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in Asia Pacific \(Sydney\) ap\-southeast\-2<a name="ap-southeast-2-instances"></a>


| Instance Type | apse2\-az1 | apse2\-az2 | apse2\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | None | 
| ml\.t3\.large | Notebook | Notebook | None | 
| ml\.t3\.xlarge | Notebook | Notebook | None | 
| ml\.t3\.2xlarge | Notebook | Notebook | None | 
| ml\.m4\.xlarge | All | All | All | 
| ml\.m4\.2xlarge | All | All | All | 
| ml\.m4\.4xlarge | All | All | All | 
| ml\.m4\.10xlarge | All | All | All | 
| ml\.m4\.16xlarge | All | All | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | None | 
| ml\.m5\.xlarge | All | All | None | 
| ml\.m5\.2xlarge | All | All | None | 
| ml\.m5\.4xlarge | All | All | None | 
| ml\.m5\.12xlarge | All | All | None | 
| ml\.m5\.24xlarge | All | All | None | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c4\.xlarge | All | All | Notebook, Training, Batch | 
| ml\.c4\.2xlarge | All | All | Notebook, Training, Batch | 
| ml\.c4\.4xlarge | All | All | All | 
| ml\.c4\.8xlarge | All | All | All | 
| ml\.c5\.large | Endpoint | Endpoint | None | 
| ml\.c5\.xlarge | All | All | None | 
| ml\.c5\.2xlarge | All | All | None | 
| ml\.c5\.4xlarge | All | All | None | 
| ml\.c5\.9xlarge | All | All | None | 
| ml\.c5\.18xlarge | All | All | None | 
| ml\.c5d\.xlarge | Notebook | Notebook | None | 
| ml\.c5d\.2xlarge | Notebook | Notebook | None | 
| ml\.c5d\.4xlarge | Notebook | Notebook | None | 
| ml\.c5d\.9xlarge | Notebook | Notebook | None | 
| ml\.c5d\.18xlarge | Notebook | Notebook | None | 
| ml\.p2\.xlarge | All | All | None | 
| ml\.p2\.8xlarge | All | Notebook, Training, Batch | None | 
| ml\.p2\.16xlarge | Notebook, Training, Batch | All | None | 
| ml\.p3\.2xlarge | Notebook, Training, Batch | None | None | 
| ml\.p3\.8xlarge | Notebook, Training, Batch | Batch | None | 
| ml\.p3\.16xlarge | Notebook, Training, Batch | Batch | None | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in Asia Pacific \(Tokyo\) ap\-northeast\-1<a name="ap-northeast-1-instances"></a>


| Instance Type | apne1\-az1 | apne1\-az2 | apne1\-az3 | apne1\-az4 | 
| --- | --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | None | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | None | Notebook | 
| ml\.t3\.large | Notebook | Notebook | None | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | None | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | None | Notebook | 
| ml\.m4\.xlarge | All | All | Endpoint | All | 
| ml\.m4\.2xlarge | All | All | Endpoint | All | 
| ml\.m4\.4xlarge | All | All | Endpoint | All | 
| ml\.m4\.10xlarge | All | All | Endpoint | All | 
| ml\.m4\.16xlarge | All | All | Endpoint | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | None | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | None | All | 
| ml\.m5\.2xlarge | All | All | None | All | 
| ml\.m5\.4xlarge | All | All | None | All | 
| ml\.m5\.12xlarge | All | All | None | All | 
| ml\.m5\.24xlarge | All | All | None | All | 
| ml\.r5\.large | None | None | None | None | 
| ml\.r5\.xlarge | None | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | None | Endpoint | 
| ml\.c4\.xlarge | All | Notebook, Endpoint | Endpoint | All | 
| ml\.c4\.2xlarge | All | Notebook, Endpoint | Endpoint | All | 
| ml\.c4\.4xlarge | All | Notebook, Endpoint | None | All | 
| ml\.c4\.8xlarge | All | Notebook, Endpoint | None | All | 
| ml\.c5\.large | Endpoint | Endpoint | None | Endpoint | 
| ml\.c5\.xlarge | All | All | None | All | 
| ml\.c5\.2xlarge | All | All | None | All | 
| ml\.c5\.4xlarge | All | All | None | All | 
| ml\.c5\.9xlarge | All | All | None | All | 
| ml\.c5\.18xlarge | All | All | None | All | 
| ml\.c5d\.xlarge | Notebook | None | None | Notebook | 
| ml\.c5d\.2xlarge | Notebook | None | None | Notebook | 
| ml\.c5d\.4xlarge | Notebook | None | None | Notebook | 
| ml\.c5d\.9xlarge | Notebook | None | None | Notebook | 
| ml\.c5d\.18xlarge | Notebook | None | None | Notebook | 
| ml\.p2\.xlarge | All | None | None | All | 
| ml\.p2\.8xlarge | All | None | None | All | 
| ml\.p2\.16xlarge | All | None | None | All | 
| ml\.p3\.2xlarge | All | None | None | All | 
| ml\.p3\.8xlarge | All | None | None | All | 
| ml\.p3\.16xlarge | Notebook, Training, Batch | None | None | All | 
| ml\.p3dn\.24xlarge | None | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | None | 
| ml\.eia1\.medium | Notebook, Endpoint | None | None | Notebook, Endpoint | 
| ml\.eia1\.large | Notebook, Endpoint | None | None | Notebook, Endpoint | 
| ml\.eia1\.xlarge | Notebook, Endpoint | None | None | Notebook, Endpoint | 
| ml\.eia2\.medium | None | None | None | None | 
| ml\.eia2\.large | None | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | None | 

## Component Support for Instances in Canada \(Central\) ca\-central\-1<a name="ca-central-1-instances"></a>


| Instance Type | cac1\-az1 | cac1\-az2 | 
| --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | 
| ml\.m4\.xlarge | All | All | 
| ml\.m4\.2xlarge | All | All | 
| ml\.m4\.4xlarge | All | All | 
| ml\.m4\.10xlarge | All | All | 
| ml\.m4\.16xlarge | All | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | 
| ml\.m5\.2xlarge | All | All | 
| ml\.m5\.4xlarge | All | All | 
| ml\.m5\.12xlarge | All | All | 
| ml\.m5\.24xlarge | All | All | 
| ml\.r5\.large | None | None | 
| ml\.r5\.xlarge | None | None | 
| ml\.r5\.2xlarge | None | None | 
| ml\.r5\.4xlarge | None | None | 
| ml\.r5\.12xlarge | None | None | 
| ml\.r5\.24xlarge | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | 
| ml\.c4\.xlarge | All | All | 
| ml\.c4\.2xlarge | All | All | 
| ml\.c4\.4xlarge | All | All | 
| ml\.c4\.8xlarge | All | All | 
| ml\.c5\.large | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | 
| ml\.c5\.2xlarge | All | All | 
| ml\.c5\.4xlarge | All | All | 
| ml\.c5\.9xlarge | All | All | 
| ml\.c5\.18xlarge | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | 
| ml\.p2\.xlarge | None | None | 
| ml\.p2\.8xlarge | None | None | 
| ml\.p2\.16xlarge | None | None | 
| ml\.p3\.2xlarge | None | Notebook, Training, Batch | 
| ml\.p3\.8xlarge | None | Notebook, Training, Batch | 
| ml\.p3\.16xlarge | None | Notebook, Training, Batch | 
| ml\.p3dn\.24xlarge | None | None | 
| ml\.g4dn\.xlarge | None | None | 
| ml\.g4dn\.2xlarge | None | None | 
| ml\.g4dn\.4xlarge | None | None | 
| ml\.g4dn\.8xlarge | None | None | 
| ml\.g4dn\.12xlarge | None | None | 
| ml\.g4dn\.16xlarge | None | None | 
| ml\.eia1\.medium | None | None | 
| ml\.eia1\.large | None | None | 
| ml\.eia1\.xlarge | None | None | 
| ml\.eia2\.medium | None | None | 
| ml\.eia2\.large | None | None | 
| ml\.eia2\.xlarge | None | None | 

## Component Support for Instances in EU \(Frankfurt\) eu\-central\-1<a name="eu-central-1-instances"></a>


| Instance Type | euc1\-az1 | euc1\-az2 | euc1\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.m4\.xlarge | All | All | All | 
| ml\.m4\.2xlarge | All | All | All | 
| ml\.m4\.4xlarge | All | All | All | 
| ml\.m4\.10xlarge | All | All | All | 
| ml\.m4\.16xlarge | All | All | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c4\.xlarge | All | All | All | 
| ml\.c4\.2xlarge | All | All | All | 
| ml\.c4\.4xlarge | All | All | All | 
| ml\.c4\.8xlarge | All | All | All | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | All | 
| ml\.c5\.2xlarge | All | All | All | 
| ml\.c5\.4xlarge | All | All | All | 
| ml\.c5\.9xlarge | All | All | All | 
| ml\.c5\.18xlarge | All | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | Notebook | 
| ml\.p2\.xlarge | None | All | Notebook, Training, Batch | 
| ml\.p2\.8xlarge | None | Notebook, Training, Batch | Notebook, Training, Batch | 
| ml\.p2\.16xlarge | Training | Training, Batch | Training, Batch | 
| ml\.p3\.2xlarge | None | All | All | 
| ml\.p3\.8xlarge | None | All | All | 
| ml\.p3\.16xlarge | None | All | All | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in EU \(Ireland\) eu\-west\-1<a name="eu-west-1-instances"></a>


| Instance Type | euw1\-az1 | euw1\-az2 | euw1\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.m4\.xlarge | All | All | All | 
| ml\.m4\.2xlarge | All | All | All | 
| ml\.m4\.4xlarge | All | Notebook, Training, Batch | All | 
| ml\.m4\.10xlarge | All | All | All | 
| ml\.m4\.16xlarge | All | All | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c4\.xlarge | All | All | All | 
| ml\.c4\.2xlarge | All | All | All | 
| ml\.c4\.4xlarge | All | All | All | 
| ml\.c4\.8xlarge | All | All | All | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | All | 
| ml\.c5\.2xlarge | All | All | All | 
| ml\.c5\.4xlarge | All | All | All | 
| ml\.c5\.9xlarge | All | All | All | 
| ml\.c5\.18xlarge | All | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | Notebook | 
| ml\.p2\.xlarge | All | All | All | 
| ml\.p2\.8xlarge | All | Notebook, Training, Batch | All | 
| ml\.p2\.16xlarge | All | All | Notebook, Training, Batch | 
| ml\.p3\.2xlarge | None | All | All | 
| ml\.p3\.8xlarge | None | All | All | 
| ml\.p3\.16xlarge | None | All | All | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.eia1\.large | None | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.eia1\.xlarge | None | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.eia2\.medium | None | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.eia2\.large | None | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.eia2\.xlarge | None | Notebook, Endpoint | Notebook, Endpoint | 

## Component Support for Instances in EU \(London\) eu\-west\-2<a name="eu-west-2-instances"></a>


| Instance Type | euw2\-az1 | euw2\-az2 | euw2\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.m4\.xlarge | All | All | All | 
| ml\.m4\.2xlarge | All | All | All | 
| ml\.m4\.4xlarge | All | All | All | 
| ml\.m4\.10xlarge | All | All | All | 
| ml\.m4\.16xlarge | All | All | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c4\.xlarge | All | All | All | 
| ml\.c4\.2xlarge | All | All | All | 
| ml\.c4\.4xlarge | All | All | All | 
| ml\.c4\.8xlarge | All | All | All | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | All | 
| ml\.c5\.2xlarge | All | All | All | 
| ml\.c5\.4xlarge | All | All | All | 
| ml\.c5\.9xlarge | All | All | All | 
| ml\.c5\.18xlarge | All | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | Notebook | 
| ml\.p2\.xlarge | None | None | None | 
| ml\.p2\.8xlarge | None | None | None | 
| ml\.p2\.16xlarge | None | None | None | 
| ml\.p3\.2xlarge | None | All | All | 
| ml\.p3\.8xlarge | None | All | All | 
| ml\.p3\.16xlarge | None | All | All | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in EU \(Paris\) eu\-west\-3<a name="eu-west-3-instances"></a>


| Instance Type | euw3\-az1 | euw3\-az2 | euw3\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.m4\.xlarge | None | None | None | 
| ml\.m4\.2xlarge | None | None | None | 
| ml\.m4\.4xlarge | None | None | None | 
| ml\.m4\.10xlarge | None | None | None | 
| ml\.m4\.16xlarge | None | None | None | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | None | None | None | 
| ml\.c4\.xlarge | None | None | None | 
| ml\.c4\.2xlarge | None | None | None | 
| ml\.c4\.4xlarge | None | None | None | 
| ml\.c4\.8xlarge | None | None | None | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | All | 
| ml\.c5\.2xlarge | All | All | All | 
| ml\.c5\.4xlarge | All | All | All | 
| ml\.c5\.9xlarge | All | All | All | 
| ml\.c5\.18xlarge | All | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | Notebook | 
| ml\.p2\.xlarge | None | None | None | 
| ml\.p2\.8xlarge | None | None | None | 
| ml\.p2\.16xlarge | None | None | None | 
| ml\.p3\.2xlarge | None | None | None | 
| ml\.p3\.8xlarge | None | None | None | 
| ml\.p3\.16xlarge | None | None | None | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in EU \(Stockholm\) eu\-north\-1<a name="eu-north-1-instances"></a>


| Instance Type | eun1\-az1 | eun1\-az2 | eun1\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | None | None | None | 
| ml\.t2\.large | None | None | None | 
| ml\.t2\.xlarge | None | None | None | 
| ml\.t2\.2xlarge | None | None | None | 
| ml\.t3\.medium | Notebook | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.m4\.xlarge | None | None | None | 
| ml\.m4\.2xlarge | None | None | None | 
| ml\.m4\.4xlarge | None | None | None | 
| ml\.m4\.10xlarge | None | None | None | 
| ml\.m4\.16xlarge | None | None | None | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | None | None | None | 
| ml\.c4\.xlarge | None | None | None | 
| ml\.c4\.2xlarge | None | None | None | 
| ml\.c4\.4xlarge | None | None | None | 
| ml\.c4\.8xlarge | None | None | None | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | All | 
| ml\.c5\.2xlarge | All | All | All | 
| ml\.c5\.4xlarge | All | All | All | 
| ml\.c5\.9xlarge | All | All | All | 
| ml\.c5\.18xlarge | All | All | All | 
| ml\.c5d\.xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.4xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.9xlarge | Notebook | Notebook | Notebook | 
| ml\.c5d\.18xlarge | Notebook | Notebook | Notebook | 
| ml\.p2\.xlarge | None | None | None | 
| ml\.p2\.8xlarge | None | None | None | 
| ml\.p2\.16xlarge | None | None | None | 
| ml\.p3\.2xlarge | None | None | None | 
| ml\.p3\.8xlarge | None | None | None | 
| ml\.p3\.16xlarge | None | None | None | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in Middle East \(Bahrain\) me\-south\-1<a name="me-south-1-instances"></a>


| Instance Type | mes1\-az1 | mes1\-az2 | mes1\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | None | None | None | 
| ml\.t2\.large | None | None | None | 
| ml\.t2\.xlarge | None | None | None | 
| ml\.t2\.2xlarge | None | None | None | 
| ml\.t3\.medium | Notebook | Notebook | Notebook | 
| ml\.t3\.large | Notebook | Notebook | Notebook | 
| ml\.t3\.xlarge | Notebook | Notebook | Notebook | 
| ml\.t3\.2xlarge | Notebook | Notebook | Notebook | 
| ml\.m4\.xlarge | None | None | None | 
| ml\.m4\.2xlarge | None | None | None | 
| ml\.m4\.4xlarge | None | None | None | 
| ml\.m4\.10xlarge | None | None | None | 
| ml\.m4\.16xlarge | None | None | None | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | None | None | None | 
| ml\.c4\.xlarge | None | None | None | 
| ml\.c4\.2xlarge | None | None | None | 
| ml\.c4\.4xlarge | None | None | None | 
| ml\.c4\.8xlarge | None | None | None | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | Notebook, Training, Endpoint | Notebook, Training, Endpoint | Notebook, Training, Endpoint | 
| ml\.c5\.2xlarge | Notebook, Training, Endpoint | Notebook, Training, Endpoint | Notebook, Training, Endpoint | 
| ml\.c5\.4xlarge | Notebook, Training, Endpoint | Notebook, Training, Endpoint | Notebook, Training, Endpoint | 
| ml\.c5\.9xlarge | Notebook, Training, Endpoint | Notebook, Training, Endpoint | Notebook, Training, Endpoint | 
| ml\.c5\.18xlarge | Notebook, Training, Endpoint | Notebook, Training, Endpoint | Notebook, Training, Endpoint | 
| ml\.c5d\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.4xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.9xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.18xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.p2\.xlarge | None | None | None | 
| ml\.p2\.8xlarge | None | None | None | 
| ml\.p2\.16xlarge | None | None | None | 
| ml\.p3\.2xlarge | None | None | None | 
| ml\.p3\.8xlarge | None | None | None | 
| ml\.p3\.16xlarge | None | None | None | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in South America \(Sao Paulo\) sa\-east\-1<a name="sa-east-1-instances"></a>


| Instance Type | sae1\-az1 | sae1\-az2 | sae1\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Notebook | None | Notebook | 
| ml\.t3\.large | Notebook | None | Notebook | 
| ml\.t3\.xlarge | Notebook | None | Notebook | 
| ml\.t3\.2xlarge | Notebook | None | Notebook | 
| ml\.m4\.xlarge | All | Endpoint | All | 
| ml\.m4\.2xlarge | All | Endpoint | All | 
| ml\.m4\.4xlarge | All | Endpoint | All | 
| ml\.m4\.10xlarge | All | None | All | 
| ml\.m4\.16xlarge | All | Endpoint | All | 
| ml\.m5\.large | Training, Batch, Endpoint | None | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | None | All | 
| ml\.m5\.2xlarge | All | None | All | 
| ml\.m5\.4xlarge | All | None | All | 
| ml\.m5\.12xlarge | All | None | All | 
| ml\.m5\.24xlarge | All | None | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | None | Endpoint | 
| ml\.c4\.xlarge | All | None | All | 
| ml\.c4\.2xlarge | All | None | All | 
| ml\.c4\.4xlarge | All | None | All | 
| ml\.c4\.8xlarge | All | None | All | 
| ml\.c5\.large | Endpoint | None | Endpoint | 
| ml\.c5\.xlarge | All | None | All | 
| ml\.c5\.2xlarge | All | None | All | 
| ml\.c5\.4xlarge | All | None | All | 
| ml\.c5\.9xlarge | All | None | All | 
| ml\.c5\.18xlarge | All | None | All | 
| ml\.c5d\.xlarge | None | None | None | 
| ml\.c5d\.2xlarge | None | None | None | 
| ml\.c5d\.4xlarge | None | None | None | 
| ml\.c5d\.9xlarge | None | None | None | 
| ml\.c5d\.18xlarge | None | None | None | 
| ml\.p2\.xlarge | None | None | None | 
| ml\.p2\.8xlarge | None | None | None | 
| ml\.p2\.16xlarge | None | None | None | 
| ml\.p3\.2xlarge | None | None | None | 
| ml\.p3\.8xlarge | None | None | None | 
| ml\.p3\.16xlarge | None | None | None | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 

## Component Support for Instances in AWS GovCloud \(US\-Gov\-West\) us\-gov\-west\-1<a name="us-gov-west-1-instances"></a>


| Instance Type | usgw1\-az1 | usgw1\-az2 | usgw1\-az3 | 
| --- | --- | --- | --- | 
| ml\.t2\.medium | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.large | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t2\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.t3\.medium | Endpoint | Endpoint | Endpoint | 
| ml\.t3\.large | Endpoint | Endpoint | Endpoint | 
| ml\.t3\.xlarge | Endpoint | Endpoint | Endpoint | 
| ml\.t3\.2xlarge | Endpoint | Endpoint | Endpoint | 
| ml\.m4\.xlarge | All | All | All | 
| ml\.m4\.2xlarge | All | All | All | 
| ml\.m4\.4xlarge | All | All | All | 
| ml\.m4\.10xlarge | Notebook, Training, Batch | Notebook, Training, Batch | All | 
| ml\.m4\.16xlarge | Notebook, Training, Batch | All | All | 
| ml\.m5\.large | Training, Batch, Endpoint | Training, Batch, Endpoint | Training, Batch, Endpoint | 
| ml\.m5\.xlarge | All | All | All | 
| ml\.m5\.2xlarge | All | All | All | 
| ml\.m5\.4xlarge | All | All | All | 
| ml\.m5\.12xlarge | All | All | All | 
| ml\.m5\.24xlarge | All | All | All | 
| ml\.r5\.large | None | None | None | 
| ml\.r5\.xlarge | None | None | None | 
| ml\.r5\.2xlarge | None | None | None | 
| ml\.r5\.4xlarge | None | None | None | 
| ml\.r5\.12xlarge | None | None | None | 
| ml\.r5\.24xlarge | None | None | None | 
| ml\.c4\.large | Endpoint | Endpoint | None | 
| ml\.c4\.xlarge | All | All | All | 
| ml\.c4\.2xlarge | All | All | All | 
| ml\.c4\.4xlarge | All | All | All | 
| ml\.c4\.8xlarge | All | All | All | 
| ml\.c5\.large | Endpoint | Endpoint | Endpoint | 
| ml\.c5\.xlarge | All | All | All | 
| ml\.c5\.2xlarge | All | All | All | 
| ml\.c5\.4xlarge | All | All | All | 
| ml\.c5\.9xlarge | All | All | All | 
| ml\.c5\.18xlarge | All | All | All | 
| ml\.c5d\.xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.2xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.4xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.9xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.c5d\.18xlarge | Notebook, Endpoint | Notebook, Endpoint | Notebook, Endpoint | 
| ml\.p2\.xlarge | Endpoint | Endpoint | Training | 
| ml\.p2\.8xlarge | Endpoint | Endpoint | Training | 
| ml\.p2\.16xlarge | Endpoint | Endpoint | Training | 
| ml\.p3\.2xlarge | Notebook, Batch, Endpoint | Notebook, Batch, Endpoint | Training | 
| ml\.p3\.8xlarge | Notebook, Batch, Endpoint | Notebook, Batch, Endpoint | Training | 
| ml\.p3\.16xlarge | Notebook, Batch | Notebook, Batch | Training | 
| ml\.p3dn\.24xlarge | None | None | None | 
| ml\.g4dn\.xlarge | None | None | None | 
| ml\.g4dn\.2xlarge | None | None | None | 
| ml\.g4dn\.4xlarge | None | None | None | 
| ml\.g4dn\.8xlarge | None | None | None | 
| ml\.g4dn\.12xlarge | None | None | None | 
| ml\.g4dn\.16xlarge | None | None | None | 
| ml\.eia1\.medium | None | None | None | 
| ml\.eia1\.large | None | None | None | 
| ml\.eia1\.xlarge | None | None | None | 
| ml\.eia2\.medium | None | None | None | 
| ml\.eia2\.large | None | None | None | 
| ml\.eia2\.xlarge | None | None | None | 