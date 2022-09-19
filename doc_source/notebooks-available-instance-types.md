# Available SageMaker Studio Instance Types<a name="notebooks-available-instance-types"></a>

The following Amazon Elastic Compute Cloud \(Amazon EC2\) instance types are available for use with SageMaker Studio notebooks\. 

For detailed information on which instance types fit your use case, and their performance capabilities, see [Amazon Elastic Compute Cloud Instance types](http://aws.amazon.com/ec2/instance-types/)\.

For information about available Amazon SageMaker Notebook Instance types, see [CreateNotebookInstance](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html#sagemaker-CreateNotebookInstance-request-InstanceType)\.

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
+ ml\.m5d\.large
+ ml\.m5d\.xlarge
+ ml\.m5d\.2xlarge
+ ml\.m5d\.4xlarge
+ ml\.m5d\.8xlarge
+ ml\.m5d\.12xlarge
+ ml\.m5d\.16xlarge
+ ml\.m5d\.24xlarge

**Compute optimized \(no GPUs\)**
+ ml\.c5\.large *>> Fast launch*
+ ml\.c5\.xlarge
+ ml\.c5\.2xlarge
+ ml\.c5\.4xlarge
+ ml\.c5\.9xlarge
+ ml\.c5\.12xlarge
+ ml\.c5\.18xlarge
+ ml\.c5\.24xlarge

**Memory optimized \(no GPUs\)**
+ ml\.r5\.large
+ ml\.r5\.xlarge
+ ml\.r5\.2xlarge
+ ml\.r5\.4xlarge
+ ml\.r5\.8xlarge
+ ml\.r5\.12xlarge
+ ml\.r5\.16xlarge
+ ml\.r5\.24xlarge

**Accelerated computing \(1\+ GPUs\)**
+ ml\.p3\.2xlarge
+ ml\.p3\.8xlarge
+ ml\.p3\.16xlarge
+ ml\.p3dn\.24xlarge
+ ml\.g4dn\.xlarge *>> Fast launch*
+ ml\.g4dn\.2xlarge
+ ml\.g4dn\.4xlarge
+ ml\.g4dn\.8xlarge
+ ml\.g4dn\.12xlarge
+ ml\.g4dn\.16xlarge
+ ml\.g5\.xlarge
+ ml\.g5\.2xlarge
+ ml\.g5\.4xlarge
+ ml\.g5\.8xlarge
+ ml\.g5\.12xlarge
+ ml\.g5\.24xlarge
+ ml\.g5\.48xlarge