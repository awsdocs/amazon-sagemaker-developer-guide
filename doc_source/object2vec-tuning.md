# Tune an Object2Vec Model<a name="object2vec-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. For the objective metric, you use one of the metrics that the algorithm computes\. Automatic model tuning searches the chosen hyperparameters to find the combination of values that result in the model that optimizes the objective metric\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the Object2Vec Algorithm<a name="object2vec-metrics"></a>

The Object2Vec algorithm has both classification and regression metrics\. The `output_layer` type determines which metric you can use for automatic model tuning\. 

### Regressor Metrics Computed by the Object2Vec Algorithm<a name="object2vec-regressor-metrics"></a>

The algorithm reports a mean squared error regressor metric, which is computed during testing and validation\. When tuning the model for regression tasks, choose this metric as the objective\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:mean\_squared\_error | Root Mean Square Error | Minimize | 
| validation:mean\_squared\_error | Root Mean Square Error | Minimize | 

### Classification Metrics Computed by the Object2Vec Algorithm<a name="object2vec--classification-metrics"></a>

The Object2Vec algorithm reports accuracy and cross\-entropy classification metrics, which are computed during test and validation\. When tuning the model for classification tasks, choose one of these as the objective\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:accuracy | Accuracy | Maximize | 
| test:cross\_entropy | Cross\-entropy | Minimize | 
| validation:accuracy | Accuracy | Maximize | 
| validation:cross\_entropy | Cross\-entropy | Minimize | 

## Tunable Object2Vec Hyperparameters<a name="object2vec-tunable-hyperparameters"></a>

You can tune the following hyperparameters for the Object2Vec algorithm\.


| Hyperparameter Name | Hyperparameter Type | Recommended Ranges and Values | 
| --- | --- | --- | 
| dropout | ContinuousParameterRange | MinValue: 0\.0, MaxValue: 1\.0 | 
| early\_stopping\_patience | IntegerParameterRange | MinValue: 1, MaxValue: 5 | 
| early\_stopping\_tolerance | ContinuousParameterRange | MinValue: 0\.001, MaxValue: 0\.1 | 
| enc\_dim | IntegerParameterRange | MinValue: 4, MaxValue: 4096 | 
| enc0\_cnn\_filter\_width | IntegerParameterRange | MinValue: 1, MaxValue: 5 | 
| enc0\_layers | IntegerParameterRange | MinValue: 1, MaxValue: 4 | 
| enc0\_token\_embedding\_dim | IntegerParameterRange | MinValue: 5, MaxValue: 300 | 
| enc1\_cnn\_filter\_width | IntegerParameterRange | MinValue: 1, MaxValue: 5 | 
| enc1\_layers | IntegerParameterRange | MinValue: 1, MaxValue: 4 | 
| enc1\_token\_embedding\_dim | IntegerParameterRange | MinValue: 5, MaxValue: 300 | 
| epochs | IntegerParameterRange | MinValue: 4, MaxValue: 20 | 
| learning\_rate | ContinuousParameterRange | MinValue: 1e\-6, MaxValue: 1\.0 | 
| mini\_batch\_size | IntegerParameterRange | MinValue: 1, MaxValue: 8192 | 
| mlp\_activation | CategoricalParameterRanges |  \[`tanh`, `relu`, `linear`\]  | 
| mlp\_dim | IntegerParameterRange | MinValue: 16, MaxValue: 1024 | 
| mlp\_layers | IntegerParameterRange | MinValue: 1, MaxValue: 4 | 
| optimizer | CategoricalParameterRanges | \[`adagrad`, `adam`, `rmsprop`, `sgd`, `adadelta`\] | 
| weight\_decay | ContinuousParameterRange | MinValue: 0\.0, MaxValue: 1\.0 | 