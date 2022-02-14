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

The following topics show how to use the model registry\.

**Topics**
+ [Create a Model Group](model-registry-model-group.md)
+ [Register a Model Version](model-registry-version.md)
+ [View Model Groups and Versions](model-registry-view.md)
+ [View the Details of a Model Version](model-registry-details.md)
+ [Update the Approval Status of a Model](model-registry-approve.md)
+ [Deploy a Model in the Registry](model-registry-deploy.md)
+ [View the Deployment History of a Model](model-registry-deploy-history.md)