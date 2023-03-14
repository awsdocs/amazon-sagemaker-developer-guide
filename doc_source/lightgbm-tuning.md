# Tune a LightGBM model<a name="lightgbm-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your training and validation datasets\. Model tuning focuses on the following hyperparameters: 

**Note**  
The learning objective function is automatically assigned based on the type of classification task, which is determined by the number of unique integers in the label column\. For more information, see [LightGBM hyperparameters](lightgbm-hyperparameters.md)\.
+ A learning objective function to optimize during model training
+ An evaluation metric that is used to evaluate model performance during validation
+ A set of hyperparameters and a range of values for each to use when tuning the model automatically

Automatic model tuning searches your specified hyperparameters to find the combination of values that results in a model that optimizes the chosen evaluation metric\.

**Note**  
Automatic model tuning for LightGBM is only available from the Amazon SageMaker SDKs, not from the SageMaker console\.

For more information about model tuning, see [Perform Automatic Model Tuning with SageMaker](automatic-model-tuning.md)\.

## Evaluation metrics computed by the LightGBM algorithm<a name="lightgbm-metrics"></a>

The SageMaker LightGBM algorithm computes the following metrics to use for model validation\. The evaluation metric is automatically assigned based on the type of classification task, which is determined by the number of unique integers in the label column\.


| Metric Name | Description | Optimization Direction | Regex Pattern | 
| --- | --- | --- | --- | 
| rmse | root mean square error | minimize | "rmse: \(\[0\-9\\\\\.\]\+\)" | 
| l1 | mean absolute error | minimize | "l1: \(\[0\-9\\\\\.\]\+\)" | 
| l2 | mean squared error | minimize | "l2: \(\[0\-9\\\\\.\]\+\)" | 
| huber | huber loss | minimize | "huber: \(\[0\-9\\\\\.\]\+\)" | 
| fair | fair loss | minimize | "fair: \(\[0\-9\\\\\.\]\+\)" | 
| binary\_logloss | binary cross entropy | maximize | "binary\_logloss: \(\[0\-9\\\\\.\]\+\)" | 
| binary\_error | binary error | minimize | "binary\_error: \(\[0\-9\\\\\.\]\+\)" | 
| auc | AUC | maximize | "auc: \(\[0\-9\\\\\.\]\+\)" | 
| average\_precision | average precision score | maximize | "average\_precision: \(\[0\-9\\\\\.\]\+\)" | 
| multi\_logloss | multiclass cross entropy | maximize | "multi\_logloss: \(\[0\-9\\\\\.\]\+\)" | 
| multi\_error | multiclass error score | minimize | "multi\_error: \(\[0\-9\\\\\.\]\+\)" | 
| auc\_mu | AUC\-mu | maximize | "auc\_mu: \(\[0\-9\\\\\.\]\+\)" | 
| cross\_entropy | cross entropy | minimize | "cross\_entropy: \(\[0\-9\\\\\.\]\+\)" | 

## Tunable LightGBM hyperparameters<a name="lightgbm-tunable-hyperparameters"></a>

Tune the LightGBM model with the following hyperparameters\. The hyperparameters that have the greatest effect on optimizing the LightGBM evaluation metrics are: `learning_rate`, `num_leaves`, `feature_fraction`, `bagging_fraction`, `bagging_freq`, `max_depth` and `min_data_in_leaf`\. For a list of all the LightGBM hyperparameters, see [LightGBM hyperparameters](lightgbm-hyperparameters.md)\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| learning\_rate | ContinuousParameterRanges | MinValue: 0\.001, MaxValue: 0\.01 | 
| num\_leaves | IntegerParameterRanges | MinValue: 10, MaxValue: 100 | 
| feature\_fraction | ContinuousParameterRanges | MinValue: 0\.1, MaxValue: 1\.0 | 
| bagging\_fraction | ContinuousParameterRanges | MinValue: 0\.1, MaxValue: 1\.0 | 
| bagging\_freq | IntegerParameterRanges | MinValue: 0, MaxValue: 10 | 
| max\_depth | IntegerParameterRanges | MinValue: 15, MaxValue: 100 | 
| min\_data\_in\_leaf | IntegerParameterRanges | MinValue: 10, MaxValue: 200 | 