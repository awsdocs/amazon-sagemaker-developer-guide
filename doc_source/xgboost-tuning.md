# Tune an XGBoost Model<a name="xgboost-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\. 

**Note**  
Automatic model tuning for XGBoost 0\.90 is only available from the SDKs, not from the Amazon SageMaker console\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the XGBoost Algorithm<a name="xgboost-metrics"></a>

The XGBoost algorithm computes the following nine metrics during training\. When tuning the model, choose one of these metrics as the objective to evaluate the model\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| validation:accuracy |  Classification rate, calculated as \#\(right\)/\#\(all cases\)\.  |  Maximize  | 
| validation:auc |  Area under the curve\.  |  Maximize  | 
| validation:error |  Binary classification error rate, calculated as \#\(wrong cases\)/\#\(all cases\)\.  |  Minimize  | 
| validation:f1 |  Indicator of classification accuracy, calculated as the harmonic mean of precision and recall\.  |  Maximize  | 
| validation:logloss |  Negative log\-likelihood\.  |  Minimize  | 
| validation:mae |  Mean absolute error\.  |  Minimize  | 
| validation:map |  Mean average precision\.  |  Maximize  | 
| validation:merror |  Multiclass classification error rate, calculated as \#\(wrong cases\)/\#\(all cases\)\.  |  Minimize  | 
| validation:mlogloss |  Negative log\-likelihood for multiclass classification\.  |  Minimize  | 
| validation:mse |  Mean squared error\.  |  Minimize  | 
| validation:ndcg |  Normalized Discounted Cumulative Gain\.  |  Maximize  | 
| validation:rmse |  Root mean square error\.  |  Minimize  | 

## Tunable XGBoost Hyperparameters<a name="xgboost-tunable-hyperparameters"></a>

Tune the open\-source XGBoost model with the following hyperparameters\. The hyperparameters that have the greatest effect on XGBoost objective metrics are: `alpha`, `min_child_weight`, `subsample`, `eta`, and `num_round`\. 


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| alpha |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 1000  | 
| colsample\_bylevel |  ContinuousParameterRanges  |  MinValue: 0\.1, MaxValue: 1  | 
| colsample\_bynode |  ContinuousParameterRanges  |  MinValue: 0\.1, MaxValue: 1  | 
| colsample\_bytree |  ContinuousParameterRanges  |  MinValue: 0\.5, MaxValue: 1  | 
| eta |  ContinuousParameterRanges  |  MinValue: 0\.1, MaxValue: 0\.5  | 
| gamma |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 5  | 
| lambda |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 1000  | 
| max\_delta\_step |  IntegerParameterRanges  |  \[0, 10\]  | 
| max\_depth |  IntegerParameterRanges  |  \[0, 10\]  | 
| min\_child\_weight |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 120  | 
| num\_round |  IntegerParameterRanges  |  \[1, 4000\]  | 
| subsample |  ContinuousParameterRanges  |  MinValue: 0\.5, MaxValue: 1  | 