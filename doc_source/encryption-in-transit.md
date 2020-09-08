# Protecting Data in Transit with Encryption<a name="encryption-in-transit"></a>

All inter\-network data in transit supports TLS 1\.2 encryption\.

Amazon SageMaker ensures that machine learning \(ML\) model artifacts and other system artifacts are encrypted in transit and at rest\. Requests to the SageMaker API and console are made over a secure \(SSL\) connection\. You pass AWS Identity and Access Management roles to SageMaker to provide permissions to access resources on your behalf for training and deployment\. You can use encrypted Amazon S3 buckets for model artifacts and data, as well as pass a AWS KMS key to SageMaker instances to encrypt the attached ML storage volumes\.

Some intra\-network data in\-transit \(inside the service platform\) is unencrypted\. This includes:
+ Command and control communications between the service control plane and training job instances \(not customer data\)\.
+ Communications between nodes in distributed processing jobs \(intra\-network\)\.
+ Communications between nodes in distributed training jobs \(intra\-network\)\.

There are no inter\-node communications for batch processing\.

You can choose to encrypt communication between nodes in a training cluster\. For information about how to do this, see [Protect Communications Between ML Compute Instances in a Distributed Training Job](train-encrypt.md)\. Enabling inter\-container traffic encryption can increase training time, especially if you are using distributed deep learning algorithms\. For affected algorithms, adding this additional level of security also increases cost\. The training time for most SageMaker built\-in algorithms, such as XGBoost, DeepAR, and linear learner, typically aren't affected\.

FIPS validated endpoints are available for the SageMaker API and request router for hosted models \(runtime\)\. For information about FIPS compliant endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](https://aws.amazon.com/compliance/fips/)\. 