# Tune an LDA Model<a name="lda-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

LDA is an unsupervised topic modeling algorithm that attempts to describe a set of observations \(documents\) as a mixture of different categories \(topics\)\. The “per\-word log\-likelihood” \(PWLL\) metric measures the likelihood that a learned set of topics \(an LDA model\) accurately describes a test document dataset\. Larger values of PWLL indicate that the test data is more likely to be described by the LDA model\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the LDA Algorithm<a name="lda-metrics"></a>

The LDA algorithm reports on a single metric during training: `test:pwll`\. When tuning a model, choose this metric as the objective metric\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:pwll | Per\-word log\-likelihood on the test dataset\. The likelihood that the test dataset is accurately described by the learned LDA model\. | Maximize | 

## Tunable LDA Hyperparameters<a name="lda-tunable-hyperparameters"></a>

You can tune the following hyperparameters for the LDA algorithm\. Both hyperparameters, `alpha0` and `num_topics`, can affect the LDA objective metric \(`test:pwll`\)\. If you don't already know the optimal values for these hyperparameters, which maximize per\-word log\-likelihood and produce an accurate LDA model, automatic model tuning can help find them\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| alpha0 | ContinuousParameterRanges | MinValue: 0\.1, MaxValue: 10 | 
| num\_topics | IntegerParameterRanges | MinValue: 1, MaxValue: 150 | 