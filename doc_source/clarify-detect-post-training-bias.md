# Detect Posttraining Data and Model Bias<a name="clarify-detect-post-training-bias"></a>

Posttraining bias analysis can help reveal biases that might have emanated from biases in the data, or from biases introduced by the classification and prediction algorithms\. These analyses take into consideration the data, including the labels, and the predictions of a model\. You assess performance by analyzing predicted labels or by comparing the predictions with the observed target values in the data with respect to groups with different attributes\. There are different notions of fairness, each requiring different bias metrics to measure\.

There are legal concepts of fairness that might not be easy to capture because they are hard to detect\. For example, the US concept of disparate impact that occurs when a group, referred to as a less favored facet * d*, experiences an adverse effect even when the approach taken appears to be fair\. This type of bias might not be due to a machine learning model, but might still be detectable by posttraining bias analysis\.

Amazon SageMaker Clarify tries to ensure a consistent use of terminology\. For a list of terms and their definitions, see [Amazon SageMaker Clarify Terms for Bias and Fairness](clarify-detect-data-bias.md#clarify-bias-and-fairness-terms)\.

For additional information about posttraining bias metrics, see [Fairness Measures for Machine Learning in Finance](https://pages.awscloud.com/rs/112-TZM-766/images/Fairness.Measures.for.Machine.Learning.in.Finance.pdf)\.

## Sample Notebooks<a name="clarify-post-training-bias-sample-notebooks"></a>

Amazon SageMaker Clarify provides the following sample notebook for posttraining bias detection:
+ [Explainability and bias detection with Amazon SageMaker Clarify](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_processing/fairness_and_explainability/fairness_and_explainability.html) – Use SageMaker Clarify to create a processing job for the detecting bias and explaining model predictions with feature attributions\.

This notebook has been verified to run in Amazon SageMaker Studio only\. If you need instructions on how to open a notebook in Amazon SageMaker Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. If you're prompted to choose a kernel, choose **Python 3 \(Data Science\)**\.

**Topics**
+ [Sample Notebooks](#clarify-post-training-bias-sample-notebooks)
+ [Measure Posttraining Data and Model Bias](clarify-measure-post-training-bias.md)
+ [Configure an Amazon SageMaker Clarify Processing Jobs for Fairness and Explainability](clarify-configure-processing-jobs.md)
+ [Run SageMaker Clarify Processing Jobs for Bias Analysis and Explainability](clarify-processing-job-run.md)
+ [Troubleshoot SageMaker Clarify Processing Jobs](clarify-processing-job-run-troubleshooting.md)