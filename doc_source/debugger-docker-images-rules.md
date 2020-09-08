# Use Debugger Docker Images for Built\-in or Custom Rules<a name="debugger-docker-images-rules"></a>

Amazon SageMaker provides two sets of Docker images for rules: one set for evaluating rules provided by SageMaker \(built\-in rules\) and one set for evaluating custom rules provided in Python source files\. 

If you use the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), you can simply use SageMaker high\-level Debugger API operations with SageMaker Estimator API operations, without having to manually retrieve the Debugger Docker images and configure the `ConfigureTrainingJob`API\. 

If you are not using the SageMaker Python SDK, you have to retrieve a relevant pre\-built container base image for the Debugger rules\. Amazon SageMaker Debugger provides pre\-built Docker images for built\-in and custom rules, and the images are stored in Amazon Elastic Container Registry \(Amazon ECR\)\. To pull an image from an Amazon ECR repository \(or to push an image to one\), use the full name registry URL of the image using the `CreateTrainingJob` API\. SageMaker uses the following URL patterns for the Debugger rule container image registry address\. 

```
<account_id>.dkr.ecr.<Region>.amazonaws.com/<ECR repository name>:<tag>
```

For the account ID in each AWS Region, Amazon ECR repository name, and tag value, see the following topics\.

**Topics**
+ [Amazon SageMaker Debugger Registry URLs for Built\-in Rule Evaluators](#debuger-built-in-registry-ids)
+ [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](#debuger-custom-rule-registry-ids)

## Amazon SageMaker Debugger Registry URLs for Built\-in Rule Evaluators<a name="debuger-built-in-registry-ids"></a>

Use the following values for the components of the registry URLs for the images that provide built\-in rules for Amazon SageMaker Debugger\. For account IDs, see the following table\.

**ECR Repository Name**: sagemaker\-debugger\-rules 

**Tag**: latest 

**Example of a full registry URL**: 

`904829902805.dkr.ecr.ap-south-1.amazonaws.com/sagemaker-debugger-rules:latest`


**Account IDs for Built\-in Rules Container Images by AWS Region**  

| Region | account\_id | 
| --- | --- | 
| ap\-east\-1 |  199566480951  | 
| ap\-northeast\-1 |  430734990657   | 
| ap\-northeast\-2 |  578805364391  | 
| ap\-south\-1 |  904829902805  | 
| ap\-southeast\-1 |  972752614525  | 
| ap\-southeast\-2 |  184798709955  | 
| ca\-central\-1 |  519511493484  | 
| cn\-north\-1 |  618459771430  | 
| cn\-northwest\-1 |  658757709296  | 
| eu\-central\-1 |  482524230118  | 
| eu\-north\-1 |  314864569078  | 
| eu\-west\-1 |  929884845733  | 
| eu\-west\-2 |  250201462417  | 
| eu\-west\-3 |  447278800020  | 
| me\-south\-1 |  986000313247  | 
| sa\-east\-1 |  818342061345  | 
| us\-east\-1 |  503895931360  | 
| us\-east\-2 |  915447279597  | 
| us\-west\-1 |  685455198987  | 
| us\-west\-2 |  895741380848  | 
| us\-gov\-west\-1 |  515509971035  | 

## Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators<a name="debuger-custom-rule-registry-ids"></a>

Use the following values for the components of the registry URL for the images that provide custom rule evaluators for Amazon SageMaker Debugger\. For account IDs, see the following table\.

**ECR Repository Name**: sagemaker\-debugger\-rule\-evaluator 

**Tag**: latest 

**Example of a full registry URL**: 

`552407032007.dkr.ecr.ap-south-1.amazonaws.com/sagemaker-debugger-rule-evaluator:latest`


**Account IDs for Custom Rules Container Images by AWS Region**  

| Region | account\_id | 
| --- | --- | 
| ap\-east\-1 |  645844755771  | 
| ap\-northeast\-1 |  670969264625   | 
| ap\-northeast\-2 |  326368420253  | 
| ap\-south\-1 |  552407032007  | 
| ap\-southeast\-1 |  631532610101  | 
| ap\-southeast\-2 |  445670767460  | 
| ca\-central\-1 |  105842248657  | 
| cn\-north\-1 |  617202126805  | 
| cn\-northwest\-1 |  658559488188  | 
| eu\-central\-1 |  691764027602  | 
| eu\-north\-1 |  091235270104  | 
| eu\-west\-1 |  606966180310  | 
| eu\-west\-2 |  074613877050  | 
| eu\-west\-3 |  224335253976  | 
| me\-south\-1 |  050406412588  | 
| sa\-east\-1 |  466516958431  | 
| us\-east\-1 |  864354269164  | 
| us\-east\-2 |  840043622174  | 
| us\-west\-1 |  952348334681  | 
| us\-west\-2 |  759209512951  | 
| us\-gov\-west\-1 |  515361955729  | 