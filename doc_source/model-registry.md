# Register and Deploy Models with Model Registry<a name="model-registry"></a>

With the SageMaker model registry you can do the following:
+ Catalog models for production\.
+ Manage model versions\.
+ Associate metadata, such as training metrics, with a model\.
+ Manage the approval status of a model\.
+ Deploy models to production\.
+ Automate model deployment with CI/CD\.

Catalog models by creating model package groups that contain different versions of a model\. You can create a model group that tracks all of the models that you train to solve a particular problem\. You can then register each model you train and the model registry adds it to the model group as a new model version\. A typical workflow might look like the following:
+ Create a model group\.
+ Create an ML pipeline that trains a model\. For information about SageMaker pipelines, see [Create and Manage SageMaker Pipelines](pipelines-build.md)\.
+ For each run of the ML pipeline, create a model version that you register in the model group you created in the first step\.

**Model Registry Structure**

The SageMaker Model Registry is structured as several model groups with model packages in each group\. Each model package in a model group corresponds to a trained model\. The version of each model package is a numerical value that starts at 1 and is incremented with each new model package added to a model group\. For example, if 5 model packages are added to a model group, the model package versions will be 1, 2, 3, 4, and 5\. The example Model Registry shown in the following image contains 3 model groups, where each group contains the model packages related to a particular ML problem\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model_registry_model_groups.png)

There are two types of model packages in SageMaker\. One type is used in the AWS Marketplace, and the other is used in the Model Registry\. Model packages used in the AWS Marketplace are not versionable entities and are not associated with model groups in the Model Registry\. For more information about model packages used in the AWS Marketplace, see [Buy and Sell Amazon SageMaker Algorithms and Models in AWS Marketplace](sagemaker-marketplace.md)\.

The model packages used in the Model Registry are versioned, and **must** be associated with a model group\. The ARN of this model package type has the structure: `'arn:aws:sagemaker:region:account:model-group/version'`

The following topics show you how to use the Model Registry\.

**Topics**
+ [Create a Model Group](model-registry-model-group.md)
+ [Register a Model Version](model-registry-version.md)
+ [View Model Groups and Versions](model-registry-view.md)
+ [View the Details of a Model Version](model-registry-details.md)
+ [Update the Approval Status of a Model](model-registry-approve.md)
+ [Deploy a Model from the Registry](model-registry-deploy.md)
+ [View the Deployment History of a Model](model-registry-deploy-history.md)