# Encrypt Output Data and Storage Volume with AWS KMS<a name="sms-security-kms-permissions"></a>

You can use AWS Key Management Service \(AWS KMS\) to encrypt output data from a labeling job by specifying a [customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys) when you create the labeling job\. If you use the API operation `CreateLabelingJob` to create a labeling job that uses automated data labeling, you can also use a customer managed key to encrypt the storage volume attached to the ML compute instances to run the training and inference jobs\.

This section describes the IAM policies you must attach to your customer managed key to enable output data encryption and the policies you must attach to your customer managed key and execution role to use storage volume encryption\. To learn more about these options, see [Output Data and Storage Volume Encryption](sms-security.md)\.

## Encrypt Output Data using KMS<a name="sms-security-kms-permissions-output-data"></a>

If you specify an AWS KMS customer managed key to encrypt output data, you must add an IAM policy similar to the following to that key\. This policy gives the IAM execution role that you use to create your labeling job permission to use this key to perform all of the actions listed in `"Action"`\. To learn more about these actions, see [AWS KMS permissions](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the AWS Key Management Service Developer Guide\.

To use this policy, replace the IAM service\-role ARN in `"Principal"` with the ARN of the execution role you use to create the labeling job\. When you create a labeling job in the console, this is the role you specify for **IAM Role** under the **Job overview** section\. When you create a labeling job using `CreateLabelingJob`, this is ARN you specify for [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-RoleArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-RoleArn)\.

```
{
    "Sid": "AllowUseOfKmsKey",
    "Effect": "Allow",
    "Principal": {
        "AWS": "arn:aws:iam::111122223333:role/service-role/example-role"
    },
    "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
    ],
    "Resource": "*"
}
```

## Encrypt Automated Data Labeling ML Compute Instance Storage Volume<a name="sms-security-kms-permissions-storage-volume"></a>

If you specify a [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobResourceConfig.html#sagemaker-Type-LabelingJobResourceConfig-VolumeKmsKeyId](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobResourceConfig.html#sagemaker-Type-LabelingJobResourceConfig-VolumeKmsKeyId) to encrypt the storage volume attached to the ML compute instance used for automated data labeling training and inference, you must do the following:
+ Attach permissions described in [Encrypt Output Data using KMS](#sms-security-kms-permissions-output-data) to the customer managed key\.
+ Attach a policy similar to the following to the IAM execution role you use to create your labeling job\. This is the IAM role you specify for [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-RoleArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-RoleArn) in `CreateLabelingJob`\. To learn more about the `"kms:CreateGrant"` action that this policy permits, see [https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html) in the AWS Key Management Service API Reference\.

```
{
"Version": "2012-10-17", 
"Statement": 
 [  
   {
    "Effect": "Allow",
    "Action": [
       "kms:CreateGrant"
    ],
    "Resource": "*"
  }
]
}
```

To learn more about Ground Truth storage volume encryption, see [Use Your KMS Key to Encrypt Automated Data Labeling Storage Volume \(API Only\)](sms-security.md#sms-security-kms-storage-volume)\.