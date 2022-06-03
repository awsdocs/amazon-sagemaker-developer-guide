# Monitor Feature Attribution Drift for Models in Production<a name="clarify-model-monitor-feature-attribution-drift"></a>

A drift in the distribution of live data for models in production can result in a corresponding drift in the feature attribution values, just as it could cause a drift in bias when monitoring bias metrics\. Amazon SageMaker Clarify feature attribution monitoring helps data scientists and ML engineers monitor predictions for feature attribution drift on a regular basis\. As the model is monitored, customers can view exportable reports and graphs detailing feature attributions in SageMaker Studio and configure alerts in Amazon CloudWatch to receive notifications if it is detected that the attribution values drift beyond a certain threshold\. 

To illustrate this with a specific situation, consider a hypothetical scenario for college admissions\. Assume that we observe the following \(aggregated\) feature attribution values in the training data and in the live data:


**College Admission Hypothetical Scenario**  

| Feature | Attribution in training data | Attribution in live data | 
| --- | --- | --- | 
| SAT score | 0\.70 | 0\.10 | 
| GPA | 0\.50 | 0\.20 | 
| Class rank | 0\.05 | 0\.70 | 

The change from training data to live data appears significant\. The feature ranking has completely reversed\. Similar to the bias drift, the feature attribution drifts might be caused by a change in the live data distribution and warrant a closer look into the model behavior on the live data\. Again, the first step in these scenarios is to raise an alarm that a drift has happened\.

We can detect the drift by comparing how the ranking of the individual features changed from training data to live data\. In addition to being sensitive to changes in ranking order, we also want to be sensitive to the raw attribution score of the features\. For instance, given two features that fall in the ranking by the same number of positions going from training to live data, we want to be more sensitive to the feature that had a higher attribution score in the training data\. With these properties in mind, we use the Normalized Discounted Cumulative Gain \(NDCG\) score for comparing the feature attributions rankings of training and live data\.

Specifically, assume we have the following:
+ *F=\[f1​,…,fm​\] * is the list of features sorted with respect to their attribution scores in the training data where *m* is the total number of features\. For instance, in our case, *F*=\[SAT Score, GPA, Class Rank\]\.
+ *a\(f\)* is a function that returns the feature attribution score on the training data given a feature *f*\. For example, *a*\(SAT Score\) = 0\.70\.
+ *F′=\[f′​1​, …, f′​m​\] *is the list of features sorted with respect to their attribution scores in the live data\. For example, *F*′= \[Class Rank, GPA, SAT Score\]\.

Then, we can compute the NDCG as:

        NDCG=DCG/iDCG​

with 
+ DCG = ∑1m*a*\(*f'i*\)/log2​\(*i*\+1\)
+ iDCG = ∑1m*a*\(*fi*\)/log2​\(*i*\+1\)

The quantity DCG measures whether features with high attribution in the training data are also ranked higher in the feature attribution computed on the live data\. The quantity iDCG measures the *ideal score* and it's just a normalizing factor to ensure that the final quantity resides in the range \[0, 1\], with 1 being the best possible value\. A NDCG value of 1 means that the feature attribution ranking in the live data is the same as the one in the training data\. In this particular example, because the ranking changed by quite a bit, the NDCG value is 0\.69\.

In SageMaker Clarify, if the NDCG value is below 0\.90, we automatically raise an alert\.

## Model Monitor Example Notebook<a name="clarify-model-monitor-sample-notebooks-feature-drift"></a>

SageMaker Clarify provides the following example notebook that shows how to capture real\-time inference data, create a baseline to monitor evolving bias against, and inspect the results: 
+ [Monitoring bias drift and feature attribution drift Amazon SageMaker Clarify](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_model_monitor/fairness_and_explainability/SageMaker-Model-Monitor-Fairness-and-Explainability.html) – Use Amazon SageMaker Model Monitor to monitor bias drift and feature attribution drift over time\.

This notebook has been verified to run in SageMaker Studio only\. If you need instructions on how to open a notebook in SageMaker Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. If you're prompted to choose a kernel, choose **Python 3 \(Data Science\)**\. The following topics contain the highlights from the last two steps, and they contain code examples from the example notebook\. 

**Topics**
+ [Model Monitor Example Notebook](#clarify-model-monitor-sample-notebooks-feature-drift)
+ [Create a SHAP Baseline for Models in Production](clarify-model-monitor-shap-baseline.md)
+ [Model Feature Attribution Drift Violations](clarify-model-monitor-model-attribution-drift-violations.md)
+ [Configure Parameters to Monitor Attribution Drift](clarify-config-json-monitor-model-explainability-parameters.md)
+ [Schedule Feature Attribute Drift Monitoring Jobs](clarify-model-monitor-feature-attribute-drift-schedule.md)
+ [Inspect Reports for Feature Attribute Drift in Production Models](clarify-feature-attribute-drift-report.md)