# Tuning a XGBoost Model<a name="xgboost-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\. 

For more information about model tuning, see [Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the XGBoost Algorithm<a name="xgboost-metrics"></a>

The XGBoost algorithm computes the following nine metrics during training\. When tuning the model, choose one of these metrics as the objective\. To avoid overfitting, we recommend tuning the model against a validation metric instead of a training metric\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| train:rmse |  Root mean square error\.  |  Minimize  | 
| train:mae |  Mean absolute error\.  |  Minimize  | 
| train:logloss |  Negative log\-likelihood\.  |  Minimize  | 
| train:error |  Binary classification error rate, calculated as \#\(wrong cases\)/\#\(all cases\)\.\.  |  Minimize  | 
| train:merror |  Multiclass classification error rate, calculated as \#\(wrong cases\)/\#\(all cases\)\.  |  Minimize  | 
| train:mlogloss |  Negative log\-likelihood for multiclass classification\.  |  Minimize  | 
| train:auc |  Area under the curve\.  |  Maximize  | 
| train:ndcg |  Normalized Discounted Cumulative Gain\.  |  Maximize  | 
| train:map |  Mean average precision\.  |  Maximize  | 
| validation:rmse |  Root mean square error\.  |  Minimize  | 
| validation:mae |  Mean absolute error\.  |  You must choose one of them as an objective to optimize when tuning the algorithm with hyperparameter values\.>Minimize  | 
| validation:logloss |  Negative log\-likelihood\.  |  Minimize  | 
| validation:error |  Binary classification error rate, calculated as \#\(wrong cases\)/\#\(all cases\)\.  |  Minimize  | 
| validation:merror |  Multiclass classification error rate, calculated as \#\(wrong cases\)/\#\(all cases\)\.  |  Minimize  | 
| validation:mlogloss |  Negative log\-likelihood for multiclass classification\.  |  Minimize  | 
| validation:auc |  Area under the curve\.  |  Maximize  | 
| validation:ndcg |  Normalized Discounted Cumulative Gain\.  |  Maximize  | 
| validation:map |  Mean average precision\.  |  Maximize  | 

## Tunable Hyperparameters<a name="xgboost-tunable-hyperparameters"></a>

Tune the XGBoost model with the following hyperparameters\. The hyperparameters that have the greatest effect on XGBoost objective metrics are: `alpha`, `min_child_weight`, `subsample`, `eta`, and `nrounds`\. 


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| num\_round |  IntegerParameterRanges  |  \[1, 4000\]  | 
| eta |  ContinuousParameterRanges  |  MinValue: 0\.1, MaxValue: 0\.5  | 
| gamma |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 5  | 
| max\_depth |  IntegerParameterRanges  |  \[0, 10\]  | 
| min\_child\_weight |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 120  | 
| max\_delta\_step |  IntegerParameterRanges  |  \[0, 10\]  | 
| subsample |  ContinuousParameterRanges  |  MinValue: 0\.5, MaxValue: 1  | 
| colsample\_bytree |  ContinuousParameterRanges  |  MinValue: 0\.5, MaxValue: 1  | 
| colsample\_bylevel |  ContinuousParameterRanges  |  MinValue: 0\.1, MaxValue: 1  | 
| lambda |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 1000  | 
| alpha |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 1000  | 