# Protect Data at Rest Using Encryption<a name="encryption-at-rest"></a>

To protect your Amazon SageMaker Studio notebooks and SageMaker notebook instances, along with your model\-building data and model artifacts, SageMaker uses the AWS Key Management Service \(AWS KMS\) to encrypt the notebooks and data\. SageMaker uses AWS managed customer master keys \(CMKs\) by default\. For more control, you can specify your own customer managed CMKs\. For more information on AWS KMS, see [What is AWS Key Management Service?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.

**Topics**
+ [Studio notebooks](encryption-at-rest-studio.md)
+ [Notebook instances and SageMaker jobs](encryption-at-rest-nbi.md)