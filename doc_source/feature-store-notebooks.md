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
|   [ Encrypt Data in your Online or Offline Feature Store using KMS key](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/feature_store_kms_key_encryption.html)  |   An advanced example on how to encrypt and decrypt data in an online or offline Feature Store using KMS key and how to verify that your data is encrypted\.   | 