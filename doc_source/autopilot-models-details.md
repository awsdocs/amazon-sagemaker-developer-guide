# View model details<a name="autopilot-models-details"></a>

Autopilot generates details about the candidate models that you can obtain\. These details include the following:
+ A plot of the aggregated SHAP values that indicate the importance of each feature\. This helps explain your models predictions\.
+ The summary statistics for various training and validation metrics, including the objective metric\.
+ A list of the hyperparameters used to train and tune the model\.

To view model details after running an Autopilot job, follow these steps:

1. Choose the **Home** icon ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png) from the left navigation pane to view the top\-level **Amazon SageMaker Studio** navigation menu\.

1. Select the **AutoML** card from the main working area\. This opens a new **Autopilot** tab\.

1. In the **Name** section, select the Autopilot job that has the details that you want to examine\. This opens a new **Autopilot job** tab\.

1. The **Autopilot job** panel lists the metric values including the **Objective** metric for each model under **Model name**\. The **Best model** is listed at the top of the list under **Model name** and is also highlighted in the **Models** tab\.

   1. To review model details, select the model that you are interested in and select **View model details**\. This opens a new **Model Details** tab\.

1. The **Model Details** tab is divided into four subsections\.

   1. The top of the **Explainability** tab contains a plot of aggregated SHAP values that indicate the importance of each feature\. Following that are the metrics and hyperparameter values for this model\. 

   1.  The **Performance** tab contains metrics statistics a confusion matrix\. 

   1. The **Artifacts** tab contains information about model inputs, outputs, and intermediate results\.

   1. The **Network** tab summarizes your network isolation and encryption choices\.
**Note**  
Feature importance and information in the **Performance** tab is only generated for the **Best model**\.

   For more information about how the SHAP values help explain predictions based on feature importance, see the whitepaper [Understanding the model explainability](https://pages.awscloud.com/rs/112-TZM-766/images/Amazon.AI.Fairness.and.Explainability.Whitepaper.pdf)\. Additional information is also available in the [Amazon SageMaker Clarify Model Explainability](clarify-model-explainability.md) topic in the SageMaker Developer Guide\. 

1. To share your Autopilot model with another SageMaker Canvas user, choose **Share Model**\. That button is located at the top right of the **Model Details** tab\.

   1. In the **Add Canvas users** section, use the down arrow to select a SageMaker Canvas user\.