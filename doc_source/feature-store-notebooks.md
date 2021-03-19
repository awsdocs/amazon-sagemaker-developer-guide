# Amazon SageMaker Feature Store Notebook Examples<a name="feature-store-notebooks"></a>

To get started using Amazon SageMaker Feature Store, you can use an example Jupyter notebook that demonstrates the key functionalities of Feature Store\. This notebook shows how to create and configure a feature group, how to ingest data using `PutRecord` API, shows how the offline data replication is performed, uses data from Feature Store for inference and lastly performs deletion and clean up steps\. 

 To access an example Jupyter notebook that demonstrates key Feature Store capabilities, see [Fraud Detection with Amazon SageMaker Feature Store](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/sagemaker_featurestore_fraud_detection_python_sdk.html)\. To run this notebook, you must attach the following policies to your IAM execution role: `AmazonSageMakerFullAccess` and `AmazonS3FullAccess`\. 

 See [IAM Roles](https://console.aws.amazon.com/iam/home#/roles) to access your role and attach these policies\. The following screenshot illustrates how the policies appear under your IAM role after they have been attached\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-policy.png)