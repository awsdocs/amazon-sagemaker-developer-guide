# Prebuilt Amazon SageMaker Docker Images for Rules<a name="debugger-docker-images-rules"></a>

Amazon SageMaker provides two kinds of prebuilt Docker images for rules, one for evaluating Amazon SageMaker owned rules and other for evaluating your custom rules as Python source files\. 

If you’re using the Amazon SageMaker Python SDK, Amazon SageMaker rules can be used with an estimator, without having to retrieve a Docker image\. If you are not using the SDK and one of its estimators to manage the container, you have to retrieve the relevant pre\-built container\. The Amazon SageMaker prebuilt Rule Docker images are stored in Amazon Elastic Container Registry \(Amazon ECR\)\. To pull an image from an Amazon ECR repo \(or to push an image to an Amazon ECR repo\), use the full name registry address of the image\. Amazon SageMaker uses the following URL patterns for the container image registry addresses\. 

```
<account_id>.dkr.ecr.<region>.amazonaws.com/<ECR repo name>:<tag>
```

**Topics**
+ [Amazon SageMaker Built\-in Rules Registry Ids](debuger-built-in-registry-ids.md)
+ [Amazon SageMaker Custom Rule Evaluator Registry Ids](debuger-custom-rule-registry-ids.md)