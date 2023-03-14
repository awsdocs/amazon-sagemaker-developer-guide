# How SageMaker Clarify Processing Jobs Work<a name="clarify-processing-job-configure-how-it-works"></a>

You can use SageMaker Clarify to analyze your datasets and models for explainability and bias\. A Clarify processing job uses the SageMaker Clarify processing container to interact with an Amazon S3 bucket containing your input datasets\. You can also use Clarify to analyze a customer model that is deployed to a SageMaker inference endpoint\.

The following graphic shows how a Clarify processing job interacts with your input data and optionally, with a customer model\. This interaction depends on the specific type of analysis being performed\. The Clarify processing container obtains the input dataset and configuration for analysis from an S3 bucket\. For certain analysis types, including feature analysis, the Clarify processing container must send requests to the model container\. Then it retrieves the model predictions from the response that the model container sends\. After that, the Clarify processing container computes and saves analysis results to the S3 bucket\.

![\[SageMaker Clarify can analyze your data or a customer model for explainability and bias.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify/clarify-processing-job.png)

You can run a Clarify processing job at multiple stages in the lifecycle of the machine learning workflow\. Clarify can help you compute the following analysis types:
+ [Pre\-training bias metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-data-bias) can help you understand the bias in your data so that you can address it and train your model on a more fair dataset\. To run a job to analyze pre\-training bias metrics, you must provide the dataset and a JSON analysis configuration file to [Configure the Analysis](clarify-processing-job-configure-analysis.md)\.
+ [Post\-training bias metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-post-training-bias.html) can help you understand any bias introduced by an algorithm, hyperparameter choices, or any bias that wasn't apparent earlier in the flow\. Clarify uses the model predictions in addition to the data and labels to identify bias\. To run a job to analyze post\-training bias metrics, you must provide the dataset and a JSON analysis configuration file\. The configuration should include the model or endpoint name\.
+ [SHAP values for explainability](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-shapley-values.html) can help you understand what impact your feature has on what your model predicts\. This feature requires a trained model\.
+ [Partial dependence plots \(PDP\)](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-partial-dependence-plots.html) can help you understand how much your predicted target variable would change if you varied the value of one feature\. This feature requires a trained model\.

SageMaker Clarify needs model predictions to compute post\-training bias metrics and feature attributions\. You can provide an endpoint or Clarify will create an ephemeral endpoint using your model name, also known as a *shadow endpoint*\. The Clarify container deletes the shadow endpoint after the computations are completed\. At a high level, the Clarify container completes the following steps:

1. Validates inputs and parameters\.

1. Creates the shadow endpoint \(if a model name is provided\)\.

1. Loads the input dataset into a data frame\.

1. Obtains model predictions from the endpoint, if necessary\.

1. Computes bias metrics and features attributions\.

1. Deletes the shadow endpoint\.

1. Generate the analysis results\.

After the Clarify processing job is complete, the analysis results will be saved in the output location that you specified in the processing output parameter of the job\. These results include a JSON file with bias metrics and global feature attributions, a visual report, and additional files for local feature attributions\. You can download the results from the output location and view them\.