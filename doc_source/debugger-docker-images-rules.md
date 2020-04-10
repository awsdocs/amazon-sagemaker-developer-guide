# Use Amazon SageMaker Debugger Images for Built\-in or Custom Rules<a name="debugger-docker-images-rules"></a>

Amazon SageMaker provides two sets of Docker images for rules, one set for evaluating rules provided by Amazon SageMaker \(built\-in rules\) and one set for evaluating custom rules provided in Python source files\. 

If you’re using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), you can use Amazon SageMaker rules with an estimator, without having to retrieve a Docker image\. The estimator manages the container it runs in for you\. If you're not using the SDK and one of its estimators, you have to retrieve the relevant prebuilt container\. The prebuilt Docker images for rules are stored in Amazon Elastic Container Registry \(Amazon ECR\)\. To pull an image from an Amazon ECR repository \(or to push an image to one\), use the full name registry URL of the image\. Amazon SageMaker uses the following URL patterns for rules container image registry addresses\. 

```
<account_id>.dkr.ecr.<Region>.amazonaws.com/<ECR repository name>:<tag>
```

For the account\_id in each AWS Region, ECR repository name, and tag value, see the following topics\.

**Topics**
+ [Amazon SageMaker Debugger Registry URLs for Built\-in Rule Evaluators](debuger-built-in-registry-ids.md)
+ [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](debuger-custom-rule-registry-ids.md)