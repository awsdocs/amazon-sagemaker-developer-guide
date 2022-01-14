# Use a Model Package to Create a Model<a name="sagemaker-mkt-model-pkg-model"></a>

Use a model package to create a deployable model that you can use to get real\-time inferences by creating a hosted endpoint or to run batch transform jobs\. You can create a deployable model from a model package by using the Amazon SageMaker console, the low\-level SageMaker API\), or the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

**Topics**
+ [Use a Model Package to Create a Model \(Console\)](#sagemaker-mkt-model-pkg-model-console)
+ [Use a Model Package to Create a Model \(API\)](#sagemaker-mkt-model-pkg-model-api)
+ [Use a Model Package to Create a Model \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)](#sagemaker-mkt-model-pkg-model-sdk)

## Use a Model Package to Create a Model \(Console\)<a name="sagemaker-mkt-model-pkg-model-console"></a>

**To create a deployable model from a model package \(console\)**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Model packages**\.

1. Choose a model package that you created from the list on the **My model packages** tab or choose a model package that you subscribed to on the **AWS Marketplace subscriptions** tab\.

1. Choose **Create model**\.

1. For **Model name**, type a name for the model\.

1. For **IAM role**, choose an IAM role that has the required permissions to call other services on your behalf, or choose **Create a new role** to allow SageMaker to create a role that has the `AmazonSageMakerFullAccess` managed policy attached\. For information, see [SageMaker Roles](sagemaker-roles.md)\.

1. For **VPC**, choose a Amazon VPC that you want to allow the model to access\. For more information, see [Give SageMaker Hosted Endpoints Access to Resources in Your Amazon VPC](host-vpc.md)\.

1. Leave the default values for **Container input options** and **Choose model package**\.

1. For environment variables, provide the names and values of environment variables you want to pass to the model container\.

1. For **Tags**, specify one or more tags to manage the model\. Each tag consists of a key and an optional value\. Tag keys must be unique per resource\.

1. Choose **Create model**\.

After you create a deployable model, you can use it to set up an endpoint for real\-time inference or create a batch transform job to get inferences on entire datasets\. For information about hosting endpoints in SageMaker, see [Deploy Models for Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model.html)\.

## Use a Model Package to Create a Model \(API\)<a name="sagemaker-mkt-model-pkg-model-api"></a>

To use a model package to create a deployable model by using the SageMaker API, specify the name or the Amazon Resource Name \(ARN\) of the model package as the `ModelPackageName` field of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ContainerDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ContainerDefinition.html) object that you pass to the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) API\.

After you create a deployable model, you can use it to set up an endpoint for real\-time inference or create a batch transform job to get inferences on entire datasets\. For information about hosted endpoints in SageMaker, see [Deploy Models for Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model.html)\.

## Use a Model Package to Create a Model \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)<a name="sagemaker-mkt-model-pkg-model-sdk"></a>

To use a model package to create a deployable model by using the SageMaker Python SDK, initialize a `ModelPackage` object, and pass the Amazon Resource Name \(ARN\) of the model package as the `model_package_arn` argument\. For example:

```
from sagemaker import ModelPackage
model = ModelPackage(role='SageMakerRole',
         model_package_arn='training-job-scikit-decision-trees-1542660466-6f92',
         sagemaker_session=sagemaker_session)
```

After you create a deployable model, you can use it to set up an endpoint for real\-time inference or create a batch transform job to get inferences on entire datasets\. For information about hosting endpoints in SageMaker, see [Deploy Models for Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model.html)\.