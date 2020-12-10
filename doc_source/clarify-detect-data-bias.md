# Detect pre\-training data bias<a name="clarify-detect-data-bias"></a>

Algorithmic bias, discrimination, fairness, and related topics have been studied across disciplines such as law, policy, and computer science\. A computer system may be considered biased if it discriminates against certain individuals or groups of individuals in favor of others\. In the context of developing unbiased machine learning systems, we are primarily concerned with applications that involve data about people\. The machine learning models powering these applications learn from data collected about people and this data often reflects demographic disparities or other inherent biases that exist in the targeted societies\. For example, the training data may not have sufficient representation of various demographic groups of interest or may contain biased labels\. The machine learning models trained on datasets that exhibit these biases could end up learning them and then reproduce or even exacerbate those biases in their predictions\. It is critical to determine whether data used for training models encodes any bias\.

Bias may be measured in before training and after training, as well as monitored against baselines after deploying models to endpoints for inference\. Pre\-training bias metrics are designed to detect and measure bias in the raw data before it is used to train a model\. The metrics used are model\-agnostic as they do not depend on any model outputs\. But there are different notions of fairness that require distinct measures of bias\. Amazon SageMaker Clarify provides eight bias metrics to quantify various fairness criteria\.

For additional information about fairness\-aware machine learning \(FAML\) pre\-training bias metrics, see [A Family of Fairness Measures for Machine Leaning in Finance](https://pages.awscloud.com/rs/112-TZM-766/images/Fairness.Measures.for.Machine.Learning.in.Finance.pdf)\.

## Amazon SageMaker Clarify terms for bias and fairness<a name="clarify-bias-and-fairness-terms"></a>

Amazon SageMaker Clarify tries to ensure a consistent use of terminology\. Here are some of the key terms used to discuss bias and fairness\.

Feature  
An individual measurable property or characteristic of a phenomenon being observed\.

Label  
Feature which is the target for training a machine learning model\. Referred to as the observed label\.

Predicted label  
The label as predicted by the model\. Also referrred to as the outcome\.

Sample  
An observed entity described by feature values and label value\.

Dataset  
A collection of samples\.

Bias  
A disproportionate weight in favor of or against an idea or thing, usually in a way that is closed\-minded, prejudicial, or unfair\. It refers to computer systems that systematically and unfairly discriminate against certain individuals or groups of individuals in favor of others\.

Fairness  
Absence of wrongful bias ensuring that results are independent of features considered to be sensitive and not related to it\. It is applied to computer systems that provide impartial and just decisions without favoring individuals or groups of individuals against others\.

Bias metric  
A function that returns numerical value\(s\) indicating the level of bias\.

Bias report  
A collection of bias metrics for a given dataset or a combination of a dataset and model\.

Positive label value\(s  
Label value that gives advantages or improvements to the entity observed in the sample\. In other words, designates a sample as having a "positive" result\. 

Group variable  
Categorical column of the dataset which is used to form subgroups for the measurement of Conditional Demographic Disparity \(CDD\)\. Required only for this metric with regards to Simpson’s paradox \(see example below, compared to female applicant, male applicants have high acceptance rate by school, but low total acceptance rate\)\.

Facet  
A column or feature that will be used to measure bias against\.

Facet value  
The feature value that requires special considerations or treatment for legal and auditing purposes\.

Predicted probability  
The probability as predicted by the model of a sample having a certain outcome\.

## Sample notebooks<a name="clarify-data-bias-sample-notebooks"></a>

Amazon SageMaker Clarify provides the following sample notebook for bias detection\.
+ [Explainability and bias detection with Amazon SageMaker Clarify](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker_processing/fairness_and_explainability/fairness_and_explainability.ipynb): Use SageMaker Clarify to create a processing job for the detecting bias and explaining model predictions with feature attributions\.

This notebook has been verified to run in Amazon SageMaker Studio only\. If you need instructions on how to open a notebook in Amazon SageMaker Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. Select the **Python 3 \(Data Science\)** kernal if prompted to choose one\.

**Topics**
+ [Amazon SageMaker Clarify terms for bias and fairness](#clarify-bias-and-fairness-terms)
+ [Sample notebooks](#clarify-data-bias-sample-notebooks)
+ [Measure pre\-training bias](clarify-measure-data-bias.md)
+ [Generate reports for bias in pre\-training data in SageMaker Studio](clarify-data-bias-reports-ui.md)