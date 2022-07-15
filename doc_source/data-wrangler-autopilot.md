# Automatically Train Models on Your Data Flow<a name="data-wrangler-autopilot"></a>

You can use Amazon SageMaker Autopilot to automatically train and tune models on the data that you've transformed in your data flow\. Amazon SageMaker Autopilot can go through several algorithms and use the one that works best with your data\. For more information about Amazon SageMaker Autopilot, see [Automate model development with Amazon SageMaker Autopilot](autopilot-automate-model-development.md)\.

When you train a model, Data Wrangler exports your data to an Amazon S3 location where Amazon SageMaker Autopilot can access it\.

You can train a model by choosing a node in your Data Wrangler flow and choosing **Export and Train** in the data preview\. You can use this method to view your dataset before you choose to train a model on it\.

You can also train a model directly from your data flow\.

The following procedure trains a model from the data flow\.

To train a model directly from your data flow, do the following\.

1. Choose the **\+** next to the node containing the training data\.

1. Choose **Train model**\.

1. \(Optional\) Specify a AWS KMS key or ID\. For more information about creating and controlling cryptographic keys to protect your data, see [AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.

1. Choose **Preview** to verify that Data Wrangler properly exported your data to Amazon SageMaker Autopilot\.

1. For **Target**, choose the target column\.

1. Choose **Create Experiment**\.

Autopilot shows you analyses about the best model's performance\. To learn more about model performance, see [Autopilot Model Insights](autopilot-model-insights.md)\.