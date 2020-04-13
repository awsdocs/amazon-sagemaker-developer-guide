# Key Management<a name="key-management"></a>

Customers can specify AWS KMS keys, including bring your own keys \(BYOK\), to use for envelope encryption with Amazon S3 input/output buckets and machine learning \(ML\) Amazon EBS volumes\. ML volumes for notebook instances and for processing, training, and hosted model Docker containers can be optionally encrypted by using AWS KMS customer\-owned keys\. All instance OS volumes are encrypted with an AWS\-managed AWS KMS key\. 

**Note**  
Certain Nitro\-based instances include local storage, dependent on the instance type\. Local storage volumes are encrypted using a hardware module on the instance\. You can't request a `VolumeKmsKeyId` when using an instance type with local storage\.  
For a list of instance types that support local instance storage, see [Instance Store Volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html#instance-store-volumes)\.  
For more information about local instance storage encryption, see [SSD Instance Store Volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ssd-instance-store.html)\.  
For more information about storage volumes on nitro\-based instances, see [Amazon EBS and NVMe on Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/nvme-ebs-volumes.html)\.

For information about AWS KMS keys see [What is AWS Key Management Service?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide*\.