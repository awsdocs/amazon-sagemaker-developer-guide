# Tune an RCF Model<a name="random-cut-forest-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning or hyperparameter optimization, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

The Amazon SageMaker RCF algorithm is an unsupervised anomaly\-detection algorithm that requires a labeled test dataset for hyperparameter optimization\. RCF calculates anomaly scores for test datapoints and then labels the datapoints as anomalous if their scores are beyond three standard deviations from the mean score\. This is known as the three\-sigma limit heuristic\. The F1\-score is based on the difference between calculated labels and actual labels\. The hyperparameter tuning job finds the model that maximizes that score\. The success of hyperparameter optimization depends on the applicability of the three\-sigma limit heuristic to the test dataset\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the RCF Algorithm<a name="random-cut-forest-metrics"></a>

The RCF algorithm computes the following metric during training\. When tuning the model, choose this metric as the objective metric\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:f1 | F1\-score on the test dataset, based on the difference between calculated labels and actual labels\. | Maximize | 

## Tunable RCF Hyperparameters<a name="random-cut-forest-tunable-hyperparameters"></a>

You can tune a RCF model with the following hyperparameters\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| num\_samples\_per\_tree | IntegerParameterRanges | MinValue: 1, MaxValue:2048 | 
| num\_trees | IntegerParameterRanges | MinValue: 50, MaxValue:1000 | 