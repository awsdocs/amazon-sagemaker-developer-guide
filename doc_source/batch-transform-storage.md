# Storage in Batch Transform<a name="batch-transform-storage"></a>

When you run a batch transform job, Amazon SageMaker attaches an Amazon Elastic Block Store storage volume to Amazon EC2 instances that process your job\. The volume stores your model, and the size of the storage volume is fixed at 30 GB\. You have the option to encrypt your model at rest in the storage volume\.

**Note**  
If you have a large model, you may encounter an `InternalServerError`\.

For more information on Amazon EBS storage and features, see the following pages:
+ [Amazon EBS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) in the Amazon EC2 User Guide for Linux Instances
+ [Amazon EBS volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volumes.html) in the Amazon EC2 User Guide for Linux Instances

**Note**  
G4dn instances come with their own local SSD storage\. To learn more about G4dn instances, see the [Amazon EC2 G4 Instances](http://aws.amazon.com/ec2/instance-types/g4/) page\.