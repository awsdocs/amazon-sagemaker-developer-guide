# How SageMaker Clarify Processing Jobs Work<a name="clarify-processing-job-configure-how-it-works"></a>

A SageMaker processing job uses the SageMaker Clarify container at several stages in the lifecycle of the machine learning workflow\. You can use the SageMaker Clarify container with your datasets and models to compute the following types of analysis:
+ [pre\-training bias metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-data-bias)
+ [post\-training bias metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-post-training-bias.html)
+ [SHAP values for explainability](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-shapley-values.html)
+ [Partial dependence plots \(PDP\)](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-partial-dependence-plots.html)

You can control which of these analyses are computed when you configure the processing job\. For pre\-training bias metrics, you need to provide the dataset\. You can compute post\-training bias metrics and explainability after your model has been trained by providing the dataset and model name\. You must configure the necessary parameters in the form of a JSON configuration file and provide this as an input to the processing job\.

After the processing job completes, the result of the analyses is saved in the output location specified in the `ProcessingOutput` parameters of the processing job\. You can then download it from there and view the outputs or you can view the results in Studio if you have run a notebook there\.

In order to compute post\-training bias metrics and SHAP values, the computation needs to get inferences for the model name provided\. To accomplish this, the processing job creates an ephemeral endpoint with the model name, known as a *shadow endpoint*\. The processing job deletes the shadow endpoint after the computations are completed\. 

At a high level, the processing job completes the following steps:

1. Validate inputs and parameters\.

1. Create the shadow endpoint\.

1. Compute pre\-training bias metrics\.

1. Compute post\-training bias\-metrics\.

1. Compute local and global feature attributions\.

1. Delete shadow endpoint\.

1. Generate output files\.