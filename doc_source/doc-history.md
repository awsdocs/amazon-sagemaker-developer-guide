# Document History for Amazon SageMaker<a name="doc-history"></a>

The following table describes the documentation for this release of Amazon SageMaker\.
+ **Latest documentation update:** November 29, 2017


****  

| Change | Description | Date | 
| --- | --- | --- | 
| New guide | This is the first release of the *Amazon SageMaker Developer Guide*\. | November 29, 2017 | 
| DeepAR algorithm | Amazon SageMaker now supports the [DeepAR Forecasting](deepar.md) algorithm\. | January 8, 2018 | 
| CloudTrail support | Amazon SageMaker now supports [logging with AWS CloudTrail\.](logging-using-cloudtrail.md) | January 11, 2018 | 
| KMS encryption support for training and hosting |  Amazon SageMaker now supports KMS encryption for hosting instances and training model artifacts at rest\. You can specify an AWS Key Management Service key that Amazon SageMaker uses to encrypt data on the storage volume attached to a hosting endpoint by using the `KmsKeyId` request parameter in a call to [CreateEndpointConfig](API_CreateEndpointConfig.md)\. You can specify an AWS KMS key that Amazon SageMaker uses to encrypt training model artifacts at rest by setting the `KmsKeyId` field of the [OutputDataConfig](API_OutputDataConfig.md) object you use to configure your training job\.  | January 17, 2018 | 
| BlazingText algorithm | Amazon SageMaker now supports the [BlazingText](blazingtext.md) algorithm\. | January 18, 2018 | 
| TensorFlow 1\.5 and MXNet 1\.0 support | Amazon SageMaker Deep Learning containers now support TensorFlow 1\.5 and Apache MXNet 1\.0\. | February 27, 2018 | 
| Application Auto Scaling support | Amazon SageMaker now supports Application Auto Scaling for prodcution variants\. | February 28, 2018 | 
| Disable direct internet access | You can now disable direct internet access for notebook instances\. For more information, see [Notebook Instances Are Enabled with Internet Access by Default](appendix-additional-considerations.md#appendix-notebook-and-internet-access)\. | March 15, 2018 | 
| Configuring notebook instances | You can use shell scripts to configure notebook instances when you create or start them\. For more information, see [Step 2\.1: \(Optional\) Customize a Notebook Instance ](notebook-lifecycle-config.md)\. | March 15, 2017 | 