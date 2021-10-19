# Deploy a Model in Amazon SageMaker<a name="how-it-works-deployment"></a>

After you train your model, you can deploy it using Amazon SageMaker to get predictions in any of the following ways:
+ To set up a persistent endpoint to get one prediction at a time, use SageMaker hosting services\.
+ To get predictions for an entire dataset, use SageMaker batch transform\.

**Topics**
+ [Deploy a Model on SageMaker Hosting Services](#how-it-works-hosting)

## Deploy a Model on SageMaker Hosting Services<a name="how-it-works-hosting"></a>

For an example of how to deploy a model to the SageMaker hosting service, see [Deploy the Model to SageMaker Hosting Services](ex1-model-deployment.md#ex1-deploy-model)\.

Or, if you prefer, watch the following video tutorial:

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/KFuc2KWrTHs?list=PLhr1KZpdzukcOr_6j_zmSrvYnLUtgqsZz/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/KFuc2KWrTHs?list=PLhr1KZpdzukcOr_6j_zmSrvYnLUtgqsZz)

SageMaker provides model hosting services for model deployment, as shown in the following diagram\. SageMaker provides an HTTPS endpoint where your machine learning model is available to provide inferences\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sagemaker-architecture.png)

