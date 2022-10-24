# Protecting Data in Transit with Encryption<a name="encryption-in-transit"></a>

All internetwork data in transit supports TLS 1\.2 encryption\.

With Amazon SageMaker, machine learning \(ML\) model artifacts and other system artifacts are encrypted in transit and at rest\. Requests to the SageMaker API and console are made over a secure \(SSL\) connection\. You pass AWS Identity and Access Management roles to SageMaker to provide permissions to access resources on your behalf for training and deployment\. Use encrypted Amazon S3 buckets for model artifacts and data, and to pass an AWS KMS key to SageMaker instances to encrypt the attached ML storage volumes\.

Some intranetwork data in transit \(inside the service platform\) is unencrypted\. This includes:
+ Command and control communications between the service control plane and training job instances \(not customer data\)\.
+ Communications between nodes in distributed processing jobs \(intranetwork\)\.
+ Communications between nodes in distributed training jobs \(intranetwork\)\.

There are no inter\-node communications for batch processing\.

You can choose to encrypt communication between nodes in a training cluster\. 

**Note**  
For use cases in the healthcare sector, the best practice for security is to encrypt communication between the nodes\.

For information about how to encrypt communication, see the next topic at [Protect Communications Between ML Compute Instances in a Distributed Training Job](train-encrypt.md)\. 

**Note**  
Encrypting inter\-container traffic can increase training time, especially if you use distributed deep learning algorithms\. For affected algorithms, this additional level of security also increases cost\. The training time for most SageMaker built\-in algorithms, such as XGBoost, DeepAR, and linear learner, typically aren't affected\.

FIPS validated endpoints are available for the SageMaker API and request router for hosted models \(runtime\)\. For information about FIPS compliant endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](https://aws.amazon.com/compliance/fips/)\. 

## Protect Communications with RStudio on Amazon SageMaker<a name="encrypt-rstudio"></a>

RStudio on Amazon SageMaker provides encryption for all communications that involve SageMaker components\. However, the previous version did not support encryption between the RStudioServerPro and RSession apps\.

RStudio released version 2022\.02\.2\-485\.pro2 in April 2022\. This version supports encryption between RStudioServerPro and RSession apps to enable end\-to\-end encryption\. The version upgrade, however, is not completely backward compatible\. As a result, you must update all of your RStudioServerPro and RSession apps\. For information about how to update your apps, see [Upgrade the RStudio Version ](rstudio-version.md)\.