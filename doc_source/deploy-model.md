# Deploy a Model<a name="deploy-model"></a>

After you build and train your model, you can deploy it to get predictions in one of two ways:
+ To set up a persistent endpoint to get one prediction at a time, use Amazon SageMaker hosting services\. For an overview on deploying a model with Amazon SageMaker hosting services, see [Deploy a Model on Amazon SageMaker Hosting Services](how-it-works-hosting.md)\.
+ To get predictions for an entire dataset, use Amazon SageMaker batch transform\. For an overview on deploying a model with Amazon SageMaker batch transform, see [Get Inferences for an Entire Dataset with Batch Transform](how-it-works-batch.md)\.

## Prerequisites<a name="deploy-model-prereqs"></a>

These topics assume that you have built and trained a machine learning model and are ready to deploy it\. If you are new to Amazon SageMaker and have not completed these prerequisite tasks, work through the steps in the [Get Started](gs.md) tutorial to familiarize yourself with an example of how Amazon SageMaker manages the data science process and how it handles model deployment\. For more information about building a model, see [Build a Model](build-model.md)\. For information about training a model, see [Train a Model](train-model.md)\.

## What do you want to do?<a name="deploy-model-tasks"></a>

Amazon SageMaker provides features to manage resources and optimize inference performance when deploying machine learning models\. For guidance on using inference pipelines, compiling and deploying models with Neo, Elastic Inference, and automatic model scaling, see the following topics\.
+ To manage data processing and real\-time predictions or to process batch transforms in a pipeline, see [Deploy an Inference Pipeline](inference-pipelines.md)\. 
+ To train TensorFlow, Apache MXNet, PyTorch, ONNX, and XGBoost models once and optimize them to deploy on ARM, Intel, and Nvidia processors, see [Amazon SageMaker Neo](neo.md)\.
+ To preprocess entire datasets quickly or to get inferences from a trained model for large datasets when you don't need a persistent endpoint, see [Batch Transform](batch-transform.md)\.
+ To speed up the throughput and decrease the latency of getting real\-time inferences from your deep learning models that are deployed as Amazon SageMaker hosted models using a GPU instance for your endpoint, see [Amazon SageMaker Elastic Inference \(EI\) ](ei.md)\.
+ To dynamically adjust the number of instances provisioned in response to changes in your workload, see [Automatically Scale Amazon SageMaker Models](endpoint-auto-scaling.md)\.

## Manage Model Deployments<a name="deploy-model-manage"></a>

For guidance on managing model deployments, including monitoring, troubleshooting, and best practices, and for information on storage associated with inference hosting instances:
+ For tools that can be used to monitor model deployments, see [Monitor Amazon SageMaker](monitoring-overview.md)\.
+ For troubleshooting model deployments, see [Troubleshoot Amazon SageMaker Model Deployments ](deploy-model-troubleshoot.md)\.
+ For model deployment best practices, see [Best Practices for Deploying Amazon SageMaker Models](best-pratices.md)\.
+ For information about the size of storage volumes provided for different sizes of hosting instances, see [Hosting Instance Storage Volumes](host-instance-storage.md)\.

## Deploy Your Own Inference Code<a name="deploy-model-advanced"></a>

For developers that need more advanced guidance on how to run your own inference code:
+ To run your own inference code hosting services, see [Use Your Own Inference Code with Hosting Services](your-algorithms-inference-code.md)\. 
+ To run your own inference code for batch transforms, see [Use Your Own Inference Code with Batch Transform](your-algorithms-batch-code.md)\.

## Guide to Amazon SageMaker<a name="deploy-model-context"></a>

[What Is Amazon SageMaker?](whatis.md)

**Topics**
+ [Prerequisites](#deploy-model-prereqs)
+ [What do you want to do?](#deploy-model-tasks)
+ [Manage Model Deployments](#deploy-model-manage)
+ [Deploy Your Own Inference Code](#deploy-model-advanced)
+ [Guide to Amazon SageMaker](#deploy-model-context)
+ [Deploy an Inference Pipeline](inference-pipelines.md)
+ [Amazon SageMaker Neo](neo.md)
+ [Batch Transform](batch-transform.md)
+ [Amazon SageMaker Elastic Inference \(EI\)](ei.md)
+ [Automatically Scale Amazon SageMaker Models](endpoint-auto-scaling.md)
+ [Monitor Amazon SageMaker](monitoring-overview.md)
+ [Troubleshoot Amazon SageMaker Model Deployments](deploy-model-troubleshoot.md)
+ [Best Practices for Deploying Amazon SageMaker Models](best-pratices.md)
+ [Hosting Instance Storage Volumes](host-instance-storage.md)