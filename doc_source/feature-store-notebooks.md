# Amazon SageMaker Feature Store Notebook Examples<a name="feature-store-notebooks"></a>

To get started using Amazon SageMaker Feature Store, you can choose from a variety of example Jupyter notebooks in the table below\. If this is your first time using Feature Store, try out the Introduction to Feature Store notebook\. To run any these notebooks, you must attach this policy to your IAM execution role: `AmazonSageMakerFeatureStoreAccess`\. 

 See [IAM Roles](https://console.aws.amazon.com/iam/home#/roles) to access your role and attach this policy\. For a walkthrough see [Adding policies to your IAM role](https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-adding-policies.html)\. The following screenshot illustrates how the policy appears under your IAM role after it has been attached\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-policy.png)

## Feature Store sample notebooks<a name="feature-store-sample-notebooks"></a>

 The following table outlines a variety of sample notebooks that address different use cases of Amazon SageMaker Feature Store\. 


| **Notebook Title** | **Description** | 
| --- | --- | 
|  [Introduction to Feature Store](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/feature_store_introduction.html)  |   An introduction to key Feature Store capabilities such as how to create, configure a feature group, and how to ingest data into an online or offline feature store\.   | 
|  [Fraud Detection with Feature Store](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/sagemaker_featurestore_fraud_detection_python_sdk.html)  |   An advanced example on how to train a fraud detection model by ingesting data into a Feature Store, querying it to form a training dataset, and how to train a simple model for inference\.   | 
|   [ Encrypt Data in your Online or Offline Feature Store using KMS key](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/feature_store_kms_key_encryption.html)  |   An advanced example on how to encrypt and decrypt data in an Online or Offline Feature Store using KMS key and how to verify that your data is encrypted\. Note that this notebook tackles encryption at rest\.  | 
|   [ Client\-side Encryption with Feature Store using AWS Encryption SDK](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/feature_store_client_side_encryption.html)  |   An advanced example how to do client\-side encryption with Feature Store using the [AWS Encryption SDK library ](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/introduction.html) which encrypts your data prior to ingesting it into your Online or Offline Feature Store\.   | 
|  [How to securely store an image dataset in Feature Store with KMS key?](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/feature_store_securely_store_images.html)  |  An advanced example that demonstrates how to securely store a dataset of images into your Feature Store using KMS key for server\-side encryption\.  | 
|  [Create a machine learning workflow from a Ground Truth classification labelling job to Feature Store](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/feature_store_classification_job_to_ground_truth.html)  |  A machine learning \(ML\) workflow that demonstrates how to feed the output of an image or text classification labelling job from AWS Ground Truth to Feature Store\.  | 

For a comprehensive set of notebooks with examples for common workflows, see [SageMaker Feature Store Workshop](https://github.com/aws-samples/amazon-sagemaker-feature-store-end-to-end-workshop)\.