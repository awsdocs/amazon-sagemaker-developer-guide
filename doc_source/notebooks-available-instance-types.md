# Available Instance Types<a name="notebooks-available-instance-types"></a>

The following Amazon Elastic Compute Cloud \(Amazon EC2\) instance types are available\. 

For detailed information on which instance types fits your use case, and their performance capabilities, see [ Amazon Elastic Compute Cloud Instance types](http://aws.amazon.com/ec2/instance-types/)\.

**Note**  
For most use cases, you should use a ml\.t3\.medium\. This is the default instance type for CPU\-based SageMaker images, and is available as part of the [AWS Free Tier](http://aws.amazon.com/free)\.

*>> Fast launch* instances types are optimized to start in under two minutes\.

**Default instance types**
+ CPU\-based images: ml\.t3\.medium *>> Fast launch*
+ GPU\-based images: ml\.g4dn\.xlarge *>> Fast launch*

**General purpose \(no GPUs\)**
+ ml\.t3\.medium *>> Fast launch*
+ ml\.t3\.large
+ ml\.t3\.xlarge
+ ml\.t3\.2xlarge
+ ml\.m5\.large *>> Fast launch*
+ ml\.m5\.xlarge
+ ml\.m5\.2xlarge
+ ml\.m5\.4xlarge
+ ml\.m5\.8xlarge
+ ml\.m5\.12xlarge
+ ml\.m5\.16xlarge
+ ml\.m5\.24xlarge

**Compute optimized \(no GPUs\)**
+ ml\.c5\.large *>> Fast launch*
+ ml\.c5\.xlarge
+ ml\.c5\.2xlarge
+ ml\.c5\.4xlarge
+ ml\.c5\.9xlarge
+ ml\.c5\.12xlarge
+ ml\.c5\.18xlarge
+ ml\.c5\.24xlarge

**Accelerated computing \(1\+ GPUs\)**
+ ml\.p3\.2xlarge
+ ml\.p3\.8xlarge
+ ml\.p3\.16xlarge
+ ml\.g4dn\.xlarge *>> Fast launch*
+ ml\.g4dn\.2xlarge
+ ml\.g4dn\.4xlarge
+ ml\.g4dn\.8xlarge
+ ml\.g4dn\.12xlarge
+ ml\.g4dn\.16xlarge