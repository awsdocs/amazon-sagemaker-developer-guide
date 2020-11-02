# Deploy Models for Inference<a name="deploy-model"></a>

After you build and train your models, you can deploy them to get predictions in one of two ways:
+ To set up a persistent endpoint to get predictions from your models, use Amazon SageMaker hosting services\. For an overview on deploying a single model or multiple models with SageMaker hosting services, see [Deploy a Model on SageMaker Hosting Services](how-it-works-deployment.md#how-it-works-hosting)\.

  For an example of how to deploy a model to the SageMaker hosting service, see [Step 6\.1: Deploy the Model to SageMaker Hosting Services](ex1-deploy-model.md)\.

  Or, if you prefer, watch the following video tutorial:  
[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/KFuc2KWrTHs?list=PLhr1KZpdzukcOr_6j_zmSrvYnLUtgqsZz/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/KFuc2KWrTHs?list=PLhr1KZpdzukcOr_6j_zmSrvYnLUtgqsZz)
+ To get predictions for an entire dataset, use SageMaker batch transform\. For an overview on deploying a model with SageMaker batch transform, see [Get Inferences for an Entire Dataset with Batch Transform](how-it-works-batch.md)\.

  For an example of how to deploy a model with batch transform, see [Step 6\.2: Deploy the Model with Batch Transform](ex1-batch-transform.md)\.

  Or, if you prefer, watch the following video tutorial:  
[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/Z9FtrRq0rc0?list=PLhr1KZpdzukcOr_6j_zmSrvYnLUtgqsZz/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/Z9FtrRq0rc0?list=PLhr1KZpdzukcOr_6j_zmSrvYnLUtgqsZz)

## Prerequisites<a name="deploy-model-prereqs"></a>

These topics assume that you have built and trained one or more machine learning models and are ready to deploy them\. If you are new to SageMaker and have not completed these prerequisite tasks, work through the steps in the [Get Started with Amazon SageMaker](gs.md) tutorial to familiarize yourself with an example of how SageMaker manages the data science process and how it handles model deployment\. For more information about training a model, see [Train Models](train-model.md)\.

## What do you want to do?<a name="deploy-model-tasks"></a>

SageMaker provides features to manage resources and optimize inference performance when deploying machine learning models\. For guidance on using inference pipelines, compiling and deploying models with Neo, Elastic Inference, and automatic model scaling, see the following topics\.
+ To manage data processing and real\-time predictions or to process batch transforms in a pipeline, see [Deploy an Inference Pipeline](inference-pipelines.md)\. 
+ If you want to deploy a model on inf1 instances, see [Compile and Deploy Models with Amazon SageMaker Neo](neo.md)\.
+ To train TensorFlow, Apache MXNet, PyTorch, ONNX, and XGBoost models once and optimize them to deploy on ARM, Intel, and Nvidia processors, see [Compile and Deploy Models with Amazon SageMaker Neo](neo.md)\.
+ To preprocess entire datasets quickly or to get inferences from a trained model for large datasets when you don't need a persistent endpoint, see [Use Batch Transform](batch-transform.md)\.
+ To speed up the throughput and decrease the latency of getting real\-time inferences from your deep learning models that are deployed as SageMaker hosted models using a GPU instance for your endpoint, see [Use Amazon SageMaker Elastic Inference \(EI\) ](ei.md)\.
+ To dynamically adjust the number of instances provisioned in response to changes in your workload, see [Automatically Scale Amazon SageMaker Models](endpoint-auto-scaling.md)\.
+ To create an endpoint that can host multiple models using a shared serving container, see [ Host Multiple Models with Multi\-Model Endpoints](multi-model-endpoints.md)\.
+ To test multiple models in production, see [Test models in production](model-ab-testing.md)\.

## Manage Model Deployments<a name="deploy-model-manage"></a>

For guidance on managing model deployments, including monitoring, troubleshooting, and best practices, and for information on storage associated with inference hosting instances:
+ For tools that can be used to monitor model deployments, see [Monitor Amazon SageMaker](monitoring-overview.md)\.
+ For troubleshooting model deployments, see [Troubleshoot Amazon SageMaker Model Deployments](deploy-model-troubleshoot.md)\.
+ For model deployment best practices, see [Deployment Best Practices](best-practices.md)\.
+ For information about the size of storage volumes provided for different sizes of hosting instances, see [Host Instance Storage Volumes](host-instance-storage.md)\.

## Deploy Your Own Inference Code<a name="deploy-model-advanced"></a>

For developers that need more advanced guidance on how to run your own inference code:
+ To run your own inference code hosting services, see [Use Your Own Inference Code with Hosting Services](your-algorithms-inference-code.md)\. 
+ To run your own inference code for batch transforms, see [Use Your Own Inference Code with Batch Transform](your-algorithms-batch-code.md)\.

## Guide to SageMaker<a name="deploy-model-context"></a>

[What Is Amazon SageMaker?](whatis.md)

**Topics**