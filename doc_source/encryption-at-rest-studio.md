# Studio notebooks<a name="encryption-at-rest-studio"></a>

In Amazon SageMaker Studio, your SageMaker Studio notebooks and data can be stored in the following locations:
+ An S3 bucket – When you onboard to Studio and enable shareable notebook resources, SageMaker shares notebook snapshots and metadata in an Amazon Simple Storage Service \(Amazon S3\) bucket\.
+ An EFS volume – When you onboard to Studio, SageMaker attaches an Amazon Elastic File System \(Amazon EFS\) volume to your domain for storing your Studio notebooks and data files\. The EFS volume persists after the domain is deleted\.
+ An EBS volume – When you open a notebook in Studio, an Amazon Elastic Block Store \(Amazon EBS\) is attached to the instance  that the notebook runs on\. The EBS volume persists for the duration of the instance\.

SageMaker uses the AWS Key Management Service \(AWS KMS\) to encrypt the S3 bucket and both volumes\. By default, it uses an AWS managed key\. For more control, you can specify your own customer managed key when you onboard to Studio or through the SageMaker API\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md) and [CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html)\.

In the `CreateDomain` API, you use the `S3KmsKeyId` parameter to specify the customer managed key for shareable notebooks\. You use the `KmsKeyId` parameter to specify the customer managed key for the EFS and EBS volumes\. The same customer managed key is used for both volumes\. The customer managed key for shareable notebooks can be the same customer managed key as used for the volumes or a different customer managed key\.