# Protect Data at Rest Using Encryption<a name="encryption-at-rest"></a>

To protect your Amazon SageMaker Studio notebooks and SageMaker notebook instances, along with your model\-building data and model artifacts, SageMaker encrypts the notebooks, as well as output from Training and Batch Transform jobs\. SageMaker encrypts these by default using the AWS Managed Key for Amazon S3\. This AWS Managed Key for Amazon S3 cannot be shared for cross\-account access\. For cross\-account access, specify your customer managed key while creating SageMaker resources so that it can be shared for cross\-account access\. For more information on AWS KMS, see [What is AWS Key Management Service?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.

**Topics**
+ [Studio notebooks](encryption-at-rest-studio.md)
+ [Notebook instances, SageMaker jobs, and Endpoints](encryption-at-rest-nbi.md)