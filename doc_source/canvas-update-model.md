# Updating a Model in Amazon SageMaker Canvas<a name="canvas-update-model"></a>

Amazon SageMaker Canvas gives you the ability to update the models that you've built using new data\. SageMaker Canvas shows you a model history, so that you can compare the models that you've built recently to those that you've generated in the past\.

Each model that you build has a version number\. The first model is Version 1\.

If you have more than one version of a model, you can delete the versions that aren't useful to you\.

You need to build at least one version of a model to add a new version or create a duplicate version\.

Use the following procedure to add a new model version\.

To add a new model version, do the following\.

1. Choose the dropdown list at the top of the page\. If you're on the first version of the model, it says **V1** at the beginning\.

1. Choose **Add version**\.

The following image visualizes the preceding procedure\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-update-model.png)

After you choose a new version, you start the process of building another model\. The process for building a new version of a model is almost the same as the process for building a model for the first time\. For new versions of a model, you can only choose datasets that have the same target column as the target column in Version 1\. For more information about building a model, see [Step 3: Build a model](canvas-getting-started.md#getting-started-step3)\.

You can use the different versions of the model to see changes in prediction accuracy when you've used different model types or data\.