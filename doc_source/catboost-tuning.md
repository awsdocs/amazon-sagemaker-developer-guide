# Tune a CatBoost model<a name="catboost-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your training and validation datasets\. Model tuning focuses on the following hyperparameters:

**Note**  
The learning loss function is automatically assigned based on the type of classification task, which is determined by the number of unique integers in the label column\. For more information, see [CatBoost hyperparameters](catboost-hyperparameters.md)\.
+ A learning loss function to optimize during model training
+ An evaluation metric that is used to evaluate model performance during validation
+ A set of hyperparameters and a range of values for each to use when tuning the model automatically

Automatic model tuning searches your chosen hyperparameters to find the combination of values that results in a model that optimizes the chosen evaluation metric\.

**Note**  
Automatic model tuning for CatBoost is only available from the Amazon SageMaker SDKs, not from the SageMaker console\.

For more information about model tuning, see [Perform Automatic Model Tuning with SageMaker](automatic-model-tuning.md)\.

## Evaluation metrics computed by the CatBoost algorithm<a name="catboost-metrics"></a>

The SageMaker CatBoost algorithm computes the following metrics to use for model validation\. The evaluation metric is automatically assigned based on the type of classification task, which is determined by the number of unique integers in the label column\. For a 


| Metric Name | Description | Optimization Direction | Regex Pattern | 
| --- | --- | --- | --- | 
| RMSE | root mean square error | minimize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| MAE | mean absolute error | minimize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| MedianAbsoluteError | median absolute error | minimize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| R2 | r2 score | maximize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| Logloss | binary cross entropy | maximize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| Precision | precision | maximize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| Recall | recall | maximize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| F1 | f1 score | maximize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| AUC | auc score | maximize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| MultiClass | multiclass cross entropy | maximize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| Accuracy | accuracy | maximize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 
| BalancedAccuracy | balanced accuracy | maximize | "bestTest = \(\[0\-9\\\\\.\]\+\)" | 

## Tunable CatBoost hyperparameters<a name="catboost-tunable-hyperparameters"></a>

Tune the CatBoost model with the following hyperparameters\. The hyperparameters that have the greatest effect on optimizing the CatBoost evaluation metrics are: `learning_rate`, `depth`, `l2_leaf_reg`, and `random_strength`\. For a list of all the CatBoost hyperparameters, see [CatBoost hyperparameters](catboost-hyperparameters.md)\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| learning\_rate | ContinuousParameterRanges | MinValue: 0\.001, MaxValue: 0\.01 | 
| depth | IntegerParameterRanges | MinValue: 4, MaxValue: 10 | 
| l2\_leaf\_reg | IntegerParameterRanges | MinValue: 2, MaxValue: 10 | 
| random\_strength | ContinuousParameterRanges | MinValue: 0, MaxValue: 10 | 