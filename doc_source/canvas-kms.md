# Encrypt Your SageMaker Canvas Data with AWS KMS<a name="canvas-kms"></a>

You might have data that you want to encrypt while using Amazon SageMaker Canvas, such as your private company information or customer data\. SageMaker Canvas uses AWS Key Management Service to protect your data\. AWS KMS is a service that you can use to create and manage cryptographic keys for encrypting your data\. For more information about AWS KMS, see [AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS KMS Developer Guide*\.

Amazon SageMaker Canvas provides you with several options for encrypting your data\. SageMaker Canvas provides default encryption within the application for tasks such as building your model and generating insights\. You can also choose to encrypt data stored in Amazon S3 to protect your data at rest\. SageMaker Canvas supports importing encrypted datasets, so you can establish an encrypted end\-to\-end workflow\. The following sections describe how you can use AWS KMS encryption to protect your data while building models with SageMaker Canvas\.

## Encrypt your data in SageMaker Canvas<a name="canvas-kms-app-data"></a>

With SageMaker Canvas, you can use two different AWS KMS encryption keys to encrypt your data in SageMaker Canvas, which you can specify when [setting up your Domain](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html)\. These two keys can be the same or different\. SageMaker Canvas uses one key for temporary application storage, visualizations, or compute purposes \(such as building models\)\. You can use either the default AWS\-managed key or specify your own\. You can also specify an optional key that SageMaker Canvas uses for long\-term storage of model objects and datasets, which are stored in the Region’s default SageMaker S3 bucket for your account\.

### Prerequisites<a name="canvas-kms-app-data-prereqs"></a>

To use your own KMS key for either of the previously described purposes, you must first give your user's IAM role permission to use the key\. Then, you can specify the KMS key when setting up your Domain\.

The simplest way to give your role permission to use the key is to modify the key policy\. Use the following procedure to give your role the necessary permissions\.

1. Open the [AWS KMS console](https://console.aws.amazon.com/kms/)\.

1. In the **Key Policy** section, choose **Switch to policy view**\.

1. Modify the key's policy to grant permissions for the `kms:GenerateDataKey` and `kms:Decrypt` actions to the IAM role\. You can add a statement that's similar to the following:

   ```
   {
     "Sid": "ExampleStmt",
     "Action": [
       "kms:Decrypt",
       "kms:GenerateDataKey"
     ],
     "Effect": "Allow",
     "Principal": {
       "AWS": "<arn:aws:iam::111122223333:role/Jane>"
     },
     "Resource": "*"
   }
   ```

1. Choose **Save changes**\.

The less preferred method is to modify the user’s IAM role to give the user permissions to use or manage the KMS key\. If you use this method, the KMS key’s policy must also allow access management through IAM\. To learn how to give permission to a KMS key through the user’s IAM role, see [Specifying KMS keys in IAM policy statements](https://docs.aws.amazon.com/kms/latest/developerguide/cmks-in-iam-policies.html) in the *AWS KMS Developer Guide*\.

### Encrypt your data in the SageMaker Canvas application<a name="canvas-kms-app-data-app"></a>

The first KMS key you can use in SageMaker Canvas is used for encrypting application data stored on Amazon Elastic Block Store \(EBS\) volumes and in the Amazon Elastic File System that SageMaker creates in your Domain\. SageMaker Canvas encrypts your data with this key in the underlying application and temporary storage systems created when using compute instances for building models and generating insights\. SageMaker Canvas passes the key to other AWS services, such as Autopilot, whenever SageMaker Canvas initiates jobs with them to process your data\.

**Note**  
For time series forecast models, SageMaker Canvas uses an AWS managed KMS key instead of a key that you specify\.

You can specify this key by setting the `KmsKeyID` in the `CreateDomain` API call or while doing the Standard Domain setup in the console\. If you don’t specify your own KMS key, SageMaker uses a default AWS managed KMS key to encrypt your data in the SageMaker Canvas application\.

To specify your own KMS key for use in the SageMaker Canvas application through the console, first set up your Amazon SageMaker Domain using the **Standard setup**\. Use the following procedure to complete the **Network and Storage Section** for the Domain\.

1. Fill out your desired Amazon VPC settings\.

1. For **Encryption key**, choose **Enter a KMS key ARN**\.

1. For **KMS ARN**, enter the ARN for your KMS key, which should have a format similar to the following: `arn:aws:kms:example-region-1:123456789098:key/111aa2bb-333c-4d44-5555-a111bb2c33dd`

### Encrypt your SageMaker Canvas data saved in Amazon S3<a name="canvas-kms-app-data-s3"></a>

The second KMS key you can specify is used for data that SageMaker Canvas stores to Amazon S3\. SageMaker Canvas saves duplicates of your input datasets, application and model data, and output data to the Region’s default SageMaker S3 bucket for your account\. The naming pattern for this bucket is `sagemaker-{region}-{account-ID}`, and SageMaker Canvas stores data in the `Canvas/` folder\.





1. Turn on **Enable notebook resource sharing**\.

1. For **S3 location for shareable notebook resources**, leave the default Amazon S3 path\. Note that SageMaker Canvas does not use this S3 path; this S3 path is used for Studio notebooks\.

1. For **Encryption key**, choose **Enter a KMS key ARN**\.

1. For **KMS ARN**, enter the ARN for your KMS key, which should have a format similar to the following: `arn:aws:kms:example-region-1:123456789098:key/111aa2bb-333c-4d44-5555-a111bb2c33dd`

## Import encrypted datasets from Amazon S3<a name="canvas-kms-datasets"></a>

Your users might have datasets that have been encrypted with a KMS key\. While the preceding section shows you how to encrypt data in SageMaker Canvas and data stored to Amazon S3, you must give your user's IAM role additional permissions if you want to import data from Amazon S3 that is already encrypted with AWS KMS\.

To give your user permissions to import encrypted datasets from Amazon S3 into SageMaker Canvas, add the following permissions to the IAM execution role that you've used for the user profile\.

```
      "kms:Encrypt",
      "kms:Decrypt",
      "kms:ReEncrypt*",
      "kms:GenerateDataKey*",
      "kms:DescribeKey"
```

To learn how to edit the IAM permissions for a role, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\. For more information about KMS keys, see [Key policies in AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS KMS Developer Guide*\.

## FAQs<a name="canvas-kms-faqs"></a>

Refer to the following FAQ items for answers to commonly asked questions about SageMaker Canvas AWS KMS support\.

### Q: Does SageMaker Canvas retain my KMS key?<a name="canvas-kms-faqs-1"></a>

A: No\. SageMaker Canvas may temporarily cache your key or pass it on to other AWS services \(such as Autopilot\), but SageMaker Canvas does not retain your KMS key\.

### Q: I specified a KMS key when setting up my Domain\. Why did my dataset fail to import in SageMaker Canvas?<a name="canvas-kms-faqs-2"></a>

A: Your user’s IAM role may not have permissions to use that KMS key\. To give your user permissions, see the [Prerequisites](#canvas-kms-app-data-prereqs)\. Another possible error is that you have a bucket policy on your Amazon S3 bucket that requires the use of a specific KMS key that doesn’t match the KMS key you specified in your Domain\. Make sure that you specify the same KMS key for your Amazon S3 bucket and your Domain\.

### Q: How do I find the Region’s default SageMaker S3 bucket for my account?<a name="canvas-kms-faqs-3"></a>

A: The default S3 bucket follows the naming pattern `sagemaker-{region}-{account-ID}`\. The `Canvas/` folder in this bucket stores your SageMaker Canvas application data\.

### Q: Can I change the default SageMaker S3 bucket used to store SageMaker Canvas data?<a name="canvas-kms-faqs-4"></a>

A: No, SageMaker creates this bucket for you\.

### Q: What does SageMaker Canvas store in the default SageMaker S3 bucket?<a name="canvas-kms-faqs-5"></a>

A: SageMaker Canvas uses the default SageMaker S3 bucket to store duplicates of your input datasets, model artifacts, and model outputs\.

### Q: What use cases are supported for using KMS keys with SageMaker Canvas?<a name="canvas-kms-faqs-6"></a>

A: With SageMaker Canvas, you can use your own encryption keys with AWS KMS for Regression, Multi\-class classification, and Binary classification models, as well as batch inference for these models\. SageMaker Canvas doesn't currently support using customer\-managed encryption keys for time series forecasting models\.