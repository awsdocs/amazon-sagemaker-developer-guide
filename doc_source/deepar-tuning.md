# Tune a DeepAR Model<a name="deepar-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the DeepAR Algorithm<a name="deepar-metrics"></a>

The DeepAR algorithm reports three metrics, which are computed during training\. When tuning a model, choose one of these as the objective\. For the objective, use either the forecast accuracy on a provided test channel \(recommended\) or the training loss\. For recommendations for the training/test split for the DeepAR algorithm, see [ Best Practices for Using the DeepAR Algorithm](deepar.md#deepar_best_practices)\. 


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:RMSE |  The root mean square error between the forecast and the actual target computed on the test set\.  |  Minimize  | 
| test:mean\_wQuantileLoss |  The average overall quantile losses computed on the test set\. To control which quantiles are used, set the `test_quantiles` hyperparameter\.   |  Minimize  | 
| train:final\_loss |  The training negative log\-likelihood loss averaged over the last training epoch for the model\.  |  Minimize  | 

## Tunable Hyperparameters for the DeepAR Algorithm<a name="deepar-tunable-hyperparameters"></a>

Tune a DeepAR model with the following hyperparameters\. The hyperparameters that have the greatest impact, listed in order from the most to least impactful, on DeepAR objective metrics are: `epochs`, `context_length`, `mini_batch_size`, `learning_rate`, and `num_cells`\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| mini\_batch\_size |  `IntegerParameterRanges`  |  MinValue: 32, MaxValue: 1028  | 
| epochs |  `IntegerParameterRanges`  |  MinValue: 1, MaxValue: 1000  | 
| context\_length |  `IntegerParameterRanges`  |  MinValue: 1, MaxValue: 200  | 
| num\_cells |  `IntegerParameterRanges`  |  MinValue: 30, MaxValue: 200  | 
| num\_layers |  `IntegerParameterRanges`  |  MinValue: 1, MaxValue: 8  | 
| dropout\_rate |  `ContinuousParameterRange`  |  MinValue: 0\.00, MaxValue: 0\.2  | 
| embedding\_dimension |  `IntegerParameterRanges`  |  MinValue: 1, MaxValue: 50  | 
| learning\_rate |  `ContinuousParameterRange`  |  MinValue: 1e\-5, MaxValue: 1e\-1  | 