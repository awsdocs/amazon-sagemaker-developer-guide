# Bring your own model to SageMaker Canvas<a name="canvas-byom"></a>

Business analysts can benefit from ML models already built by data scientists to solve business problems instead of creating a new model in Amazon SageMaker Canvas\. However, it might be difficult to use these models outside the environments in which they are built due to technical requirements, rigidity of tools, and manual processes to import models\. This often forces users to rebuild ML models, resulting in the duplication of effort and additional time and resources\.

SageMaker Canvas removes these limitations so you can generate predictions in Canvas with models that you’ve trained anywhere\. You can register ML models in [SageMaker Model Registry](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html), which is a metadata store for ML models, and import them into SageMaker Canvas\. Additionally, you can generate predictions with models that data scientists have trained in Amazon SageMaker Autopilot or SageMaker JumpStart\. Canvas users can then analyze and generate predictions from any model that has been shared with them\.

After you’ve satisfied the [Prerequisites](#canvas-byom-prereqs), see the following sections for instructions on how to bring your own models into Canvas and generate predictions\. The workflow begins in Studio, where a Studio user shares a model with a Canvas user\. Then, the Canvas user signs in to their Canvas app to receive the shared model and generate predictions with it\.

**Important**  
You can only share models trained with tabular data\. Also, you can't share time series models\.

## Prerequisites<a name="canvas-byom-prereqs"></a>

To bring your model into SageMaker Canvas, complete the following prerequisites:
+ You must have a Amazon SageMaker Studio user who has onboarded to Amazon SageMaker Domain\. The Studio user must be in the same Domain as the Canvas user\. Model sharing occurs when a Studio user shares a model with a Canvas user from within Studio\. If you don’t already have a Studio user set up, see the [Studio documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html) and [Onboard to Amazon SageMaker Domain](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html)\.
+ You must have a trained model from SageMaker Autopilot, SageMaker JumpStart, or SageMaker Model Registry\. For any model that you’ve built outside of SageMaker, you must register your model in Model Registry before importing it into Canvas\. For more information, see the [Model Registry documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html)\.
+ The Canvas user with whom you want to share your model must have permission to access the Amazon S3 bucket in which you store your datasets and model artifacts\. For instructions on how admins can give Canvas users the permissions they need, see [Grant Users Permissions to Collaborate with Studio](canvas-collaborate-permissions.md)\.
+ You should also have the user profile name of the Canvas user with whom you want to collaborate\. The Canvas user must be in the same Amazon SageMaker Domain as your Studio user\. You can find a user’s profile name by using the following procedure:

  1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

  1. In the navigation panel, choose **Control panel**\.

  1. Under **Users**, you can find all of the user profile names in the Domain\. Keep the user profile name ready for the first step of the following tutorial\.

If your SageMaker Canvas app is running in a private customer VPC, any Autopilot models shared from Studio must use Autopilot HPO mode to support generating predictions in Canvas\. For more information about HPO mode, see [Training modes and algorithm support](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-model-support-validation.html) in the Autopilot documentation\.

**Note**  
If you want feedback from data scientists on a model built inside Canvas, see [Collaborate with data scientists](canvas-collaborate.md), where a Canvas user shares a model with a Studio user, and the Studio user shares feedback or model updates\.

## Studio users: Share a model to SageMaker Canvas<a name="canvas-byom-share"></a>

You should have a model trained with tabular data that you’re ready to share with Canvas users\. See the following sections for information on how to share your models from features within Studio\.

### Autopilot<a name="canvas-byom-share-autopilot"></a>

You can share a model to Canvas from Amazon SageMaker Autopilot in Studio\. Autopilot is a feature that enables you to train and and deploy your models in SageMaker\.

You need to have a Studio user and a trained model ready to share from Autopilot\. For more information on how to set up Studio, see the [Studio documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html)\. For more information about Autopilot, see the [Autopilot documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-automate-model-development.html)\.

To share a model from Autopilot to Canvas, use the following procedure\.

1. Open your Amazon SageMaker Studio application\.

1. Choose the home icon \(![\[Home icon in Studio\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. In the side navigation bar of Studio, choose **AutoML** to open Autopilot\.

1. On the **Autopilot** page, select the Autopilot model that you want to share with the Canvas user\. You can only share one model at a time\.

1. From the Autopilot job details page, in the **Models** tab, select the model version that you want to share\.

1. Choose **Share**\.

1. In the **Share model** dialog box, do the following:

   1. For the **Add Canvas users** field, enter the Canvas user’s profile name\. You can enter up to 23 Canvas users\. If a user profile you specify doesn’t have a Canvas app associated with it, you can't enter the profile name\.

   1. For the **Add a note** field, add a description or note for the Canvas user when they receive the model\.

   1. Choose **Share** to share the model\.

You have now shared the model with the Canvas user\.

### JumpStart<a name="canvas-byom-share-jumpstart"></a>

You can share a model to Canvas from SageMaker JumpStart in Studio\. With JumpStart, you can access and tune pretrained models before deploying them\.

You need to have a Studio user and a successfully completed training job in JumpStart\. For more information about how to set up Studio, see the [Studio documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html)\. For more information about JumpStart, see the [JumpStart documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html)\.

To share a model from JumpStart to Canvas, use the following procedure\.

1. Open your Amazon SageMaker Studio application\.

1. Choose the home icon \(![\[Home icon in Studio\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. In the side navigation bar of Studio, choose **Quick start solutions**\.

1. Choose **Launched Quick start assets** to open the page that lists your JumpStart training jobs, models, and endpoints\.

1. Choose the **Training jobs** tab to view the list of your model training jobs\.

1. From the **Training jobs** list, select the training job that you want to share with the Canvas user\. You can only share one job at a time\. This opens the training job details page\.

1. In the header for the training job, choose **Share**, and select **Share to Canvas**\.
**Note**  
You can only share tabular models to Canvas\. Trying to share a model that is not tabular throws an `Unsupported data type` error\.

1. In the **Share to Canvas** dialog box, do the following:

   1. For the **Add Canvas users to share** field, enter the Canvas user’s profile name\. You can enter up to 23 Canvas users\. If a user profile you specify doesn’t have a Canvas app associated with it, you can't enter the profile name\.

   1. For the **Add a note** field, add a description or note for the Canvas user when they receive the model\.

   1. Choose **Share** to share the model\.

You have now shared the model with the Canvas user\.

### Model Registry<a name="canvas-byom-share-model-registry"></a>

You can share a model to Canvas from SageMaker Model Registry in Studio\. With Model Registry, you can register models that you bring from outside of SageMaker and integrate them with your ML pipelines\.

You need to have a Studio user and a model version saved in the Model Registry\. For more information about how to set up Studio, see the [Studio documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html)\. If you don’t have a model version in the Model Registry, create a model group and register a version to it\. For more information about Model Registry, see the [Model Registry documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html)\.

To share a model version from Model Registry to Canvas, use the following procedure\.

1. Open your Amazon SageMaker Studio application\.

1. Choose the home icon \(![\[Home icon in Studio\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. In the side navigation bar of Studio, choose **Models**\.

1. Select **Model Registry** from the dropdown list to open the Model Registry page and show all of the model groups registered in your account\.

1. Choose the model group that has the model version that you want to share\.

1. On the model group page, under **Versions**, select the model version from the list that you want to share with the Canvas user\. You can only share one model version at a time\.

1. Choose **Share**\.

1. In the **Share model** dialog box, do the following:

   1. For the **Add Canvas users to share** field, enter the Canvas user’s profile name\. You can enter up to 23 Canvas users\. If a user profile you specify doesn’t have a Canvas app associated with it, you can't enter the profile name\.

   1. For **Add model details**, do the following:

      1. For the **Training dataset** field, enter the Amazon S3 path for your training dataset\.

      1. For the **Validation dataset** field, enter the Amazon S3 path for your validation dataset\.

      1. For **Target column**, either select **Use the first column** if the first column in your dataset is the target, or select **Specify the target column name** to set the target as a different column in your dataset\.

      1. For **Column headers**, select one of the following options:

         1. Select **Use the first row** if the first row of your dataset contains the column headers\.

         1. Select **Specify a different dataset in S3 for column headers** if you have a file stored in Amazon S3 containing headers that can be mapped to your dataset\. The headers file must have the same number of columns as your dataset\.

         1. Select **Automatically generate** if you don’t already have column headers and would like SageMaker to generate generic column names for your dataset\.

      1. From the **Problem type** dropdown list, select your model type\.

      1. If you selected the **Binary classification** or **Multi\-class** problem types, the **Configure model outputs** option appears\.

         If you already have a file stored in Amazon S3 that maps default target column class names to your desired class names, then turn on **Model output names** and enter the Amazon S3 path to the mapping file\. If you don't have a mapping file, then turn off **Model output names** and manually enter the **Numer of model outputs** \(the number of target column classes in your data\)\. Then, enter your desired class names to replace the default class names\.

   1. \(Optional\) For the **Add a note** field, add a description or note for the Canvas user when they receive the model\.

   1. Choose **Share** to share the model version\.

You have now shared the model with the Canvas user\.

### Shared models and notebooks<a name="canvas-byom-shared-models"></a>

On the **Shared models and notebooks** page in Amazon SageMaker Studio, you can view the models that you've shared and that have been shared with you\. This page gives you a central place to view and manage all of your models in Studio\.

You need to have a Studio user and a model ready to share from Autopilot, JumpStart, or Model Registry\. For more information on how to set up Studio, see the [Studio documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html)\. For more information about the **Shared models and notebooks** page, see the [Shared models and notebooks documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/jumpstart-content-sharing.html)\.

The following example walks you through sharing an Amazon SageMaker Autopilot model, but you can use the sharing feature on the **Shared models and notebooks** page to share models from any of the other features in the previous sections, such as Jumpstart and Model Registry\.

To share an Autopilot model from the **Shared models and notebooks** page, use the following procedure\.

1. Open your Amazon SageMaker Studio application\.

1. Choose the home icon \(![\[Home icon in Studio\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. In the side navigation bar of Studio, choose **Models**\.

1. In the dropdown list, choose **Shared models** to open the **Shared models and notebooks** page\.

1. Choose the filter icon, and in the **Shared from** dropdown list, choose **Autopilot**\.

1. Select the Autopilot model from the list that you want to share with the Canvas user\. You can only share one model at a time\. Alternatively, you can select the model to open the model details page\.

1. From either the Autopilot jobs page or the model details page, choose **Share**\.

1. In the **Share model** dialog box, do the following:

   1. For the **Add Canvas users to share** field, enter the Canvas user’s profile name\. You can enter up to 23 Canvas users\. If a user profile you specify doesn’t have a Canvas app associated with it, you can't enter the profile name\.

   1. For the **Add a note** field, add a description or note for the Canvas user when they receive the model\.

   1. Choose **Share** to share the model\.

You have now shared the model with the Canvas user\.

After you share the model, you receive a notification popup in Studio similar to the following screenshot\.

![\[Screenshot of the Studio notification that you shared a model successfully.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/sharing/studio-shared-successfully.png)

You can choose **View model** to open the **Shared models and notebooks** page in Studio\. You can also view your shared models at any time from the **Shared models and notebooks** page\.

From this page, you can see the models that you’ve shared with the Canvas user under the **Shared by me** label, as shown in the following screenshot\.

![\[Screenshot of the Studio Shared models and notebooks page where you can view all of your shared models.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/sharing/studio-shared-by-me.png)

Models that you’ve shared to Canvas have text on the card similar to the following example: `Shared to: 12 Canvas users`\.

## Canvas users: Receive a shared model in SageMaker Canvas<a name="canvas-byom-receive"></a>

When a Studio user shares a model with a Canvas user, you receive a notification within the Canvas application that a Studio user has shared a model with you\.

In the Canvas application, the notification is similar to the following screenshot\.

![\[Screenshot of the notification message in the SageMaker Canvas application for a newly shared model.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/sharing/canvas-new-model-notification.png)

You can choose **View update** to see the shared model, or you can go to the **Models** page in the Canvas application to discover all of the models that have been shared with you\.

**Note**  
Canvas users can’t edit a model that has been shared with them by a Studio user\. Models imported from Studio are view and predict only\.

A model that has been shared by a Studio user looks like the following card on the **Models** page\. This is different from [Collaborate with data scientists](canvas-collaborate.md), where a Canvas user shares a model and a Studio user shares updates or feedback with the Canvas user\.

![\[Screenshot of the model card in the SageMaker Canvas application for a model that has been shared from Studio.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/sharing/canvas-shared-model-card.png)

The model import from Studio can take up to 20 minutes, during which the model shows as **Importing**\.

After importing the model, you can view its metrics and generate predictions with it\. SageMaker Canvas uses [Amazon SageMaker Serverless Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/serverless-endpoints.html) resources to generate model analysis and predictions for shared models\. You might see costs associated with Serverless Inference in your AWS account\.

The following screenshot shows the **Analyze** tab in the Canvas application for a shared model, where you can evaluate the model accuracy and metrics\. For more information, see [Evaluating Your Model's Performance in Amazon SageMaker Canvas](canvas-evaluate-model.md)\.

![\[Screenshot of the Analyze tab in the SageMaker Canvas application for a shared model.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/sharing/canvas-analyze-tab.png)

The following screenshot shows the **Predict** tab, where you can generate predictions with the model\. For more information on generating predictions in Canvas, see [Step 5: Make predictions](canvas-getting-started.md#canvas-getting-started-step5)\.

![\[Screenshot of the Predict tab in the SageMaker Canvas application for a shared model.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/sharing/canvas-predict-tab.png)

On both the **Analyze** and **Predict** tabs, you can see the **Shared History** panel, which shows you the model versions and comments shared with you by Studio users\.