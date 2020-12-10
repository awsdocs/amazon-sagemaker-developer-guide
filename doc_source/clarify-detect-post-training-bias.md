# Detect post\-training data and model bias<a name="clarify-detect-post-training-bias"></a>

Post\-training bias analysis can help reveal biases that may have emanated from biases in the data or from biases introduced by the classification and prediction algorithms\. The predicted probabilities from the model \(p’\) and the predicted labels \(y’\) provide additional information that allow an additional set of post\-training bias metrics to be calculated based on the model prediction behavior\. Performance is assessed by analyzing predicted labels or by comparing the predictions with the observed target values in the data with respect to advantaged and disadvantaged groups being compared for fair treatment\. There are different notions of fairness, each requiring different bias metrics to measure\.

There are legal notions of fairness that may not be easy to capture because they are hard to detect\. For example, the US notion of disparate impact that occurs when a group, referrred to as a disadvantaged facet, experiences bias even when the approach taken is apparently fair\. This type of bias may not be due to a machine leaning model, but may still be detectable by post\-training bias analysis\.

Amazon SageMaker Clarify tries to ensure a consistent use of terminology\. Here are some of the key terms used to discuss bias and fairness\. For a list of terms and their definitions, see [Amazon SageMaker Clarify terms for bias and fairness](clarify-detect-data-bias.md#clarify-bias-and-fairness-terms)

For additional information about fairness\-aware machine learning \(FAML\) post\-training bias metrics, see [A Family of Fairness Measures for Machine Leaning in Finance](https://pages.awscloud.com/rs/112-TZM-766/images/Fairness.Measures.for.Machine.Learning.in.Finance.pdf)\.

## Sample notebooks<a name="clarify-post-training-bias-sample-notebooks"></a>

Amazon SageMaker Clarify provides the following sample notebook for post\-training bias detection\.
+ [Explainability and bias detection with Amazon SageMaker Clarify](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker_processing/fairness_and_explainability/fairness_and_explainability.ipynb): Use SageMaker Clarify to create a processing job for the detecting bias and explaining model predictions with feature attributions\.

This notebook has been verified to run in Amazon SageMaker Studio only\. If you need instructions on how to open a notebook in Amazon SageMaker Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. Select the **Python 3 \(Data Science\)** kernal if prompted to choose one\.

**Topics**
+ [Sample notebooks](#clarify-post-training-bias-sample-notebooks)
+ [Measure post\-training data and model bias](clarify-measure-post-training-bias.md)
+ [Configure an Amazon SageMaker Clarify processing jobs for fairness and explainability](clarify-configure-processing-jobs.md)
+ [Run SageMaker Clarify processing jobs for bias analysis and explainability](clarify-processing-job-run.md)