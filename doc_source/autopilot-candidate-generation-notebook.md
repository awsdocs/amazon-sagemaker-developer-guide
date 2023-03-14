# Candidate definition notebook<a name="autopilot-candidate-generation-notebook"></a>

The candidate definition notebook contains each suggested preprocessing step, algorithm, and hyperparameter ranges\. 

You can choose which candidate to train and tune in two ways\. The first, by running sections of the notebook\. The second, by running the entire notebook to optimize all candidates to identify a best candidate\. If you run the entire notebook, only the best candidate is displayed after job completion\. 

**Note**  
Candidate definition notebooks are not available for [ensembling mode](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-model-support-validation.html#autopilot-candidate-generation-notebook.html)\.

To run Autopilot from SageMaker Studio, open the candidate definition notebook by following these steps:

1. Choose the **Home** icon ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png) from the left navigation pane to view the top\-level **Amazon SageMaker Studio** navigation menu\.

1. Select the **AutoML** card from the main working area\. This opens a new **Autopilot** tab\.

1. In the **Name** section, select the Autopilot job that has the candidate definition notebook that you want to examine\. This opens a new **Autopilot job** tab\.

1. Choose **Open candidate generation notebook** from the top right section of the **Autopilot job** tab\. This opens a new read\-only preview of the **Amazon SageMaker Autopilot Candidate Definition Notebook**\.

To run the candidate definition notebook, follow these steps:

1. Choose **Import notebook** at the top right of the **Amazon SageMaker Autopilot Candidate Definition Notebook** tab\. This opens a tab to set up a new notebook environment to run the notebook\.

1. Select an existing SageMaker **Image** or use a **Custom Image**\. 

1. Select a **Kernel**, an **Instance type**, and an optional **Start\-up script**\.

You can now run the notebook in this new environment\.