# CatBoost hyperparameters<a name="catboost-hyperparameters"></a>

The following table contains the subset of hyperparameters that are required or most commonly used for the Amazon SageMaker CatBoost algorithm\. Users set these parameters to facilitate the estimation of model parameters from data\. The SageMaker CatBoost algorithm is an implementation of the open\-source [CatBoost](https://github.com/catboost/catboost) package\.

The SageMaker CatBoost algorithm automatically chooses an evaluation metric and loss function based on the type of classification problem\. The CatBoost algorithm detects the type of classification problem based on the number of labels in your data\. For regression problems, the evaluation metric and loss functions are both root mean squared error\. For binary classification problems, the evaluation metric is Area Under the Curve \(AUC\) and the loss function is log loss\. For multiclass classification problems, the evaluation metric and loss functions are multiclass cross entropy\.

**Note**  
The CatBoost evaluation metric and loss functions are not currently available as hyperparameters\. Instead, the Amazon SageMaker CatBoost built\-in algorithm automatically detects the type of classification task \(regression, binary, or multiclass\) based on the number of unique integers in the label column and assigns an evaluation metric and loss function\.


| Parameter Name | Description | 
| --- | --- | 
| iterations |  The maximum number of trees that can be built\. Valid values: integer, range: \(`0`, `∞`\)\. Default value: `500`\.  | 
| early\_stopping\_rounds |  The training will stop if one metric of one validation data point does not improve in the last `early_stopping_rounds` round\. If `early_stopping_rounds` is less than or equal to zero, this hyperparameter is ignored\. Valid values: integer\. Default value: `5`\.  | 
| learning\_rate |  The rate at which the model weights are updated after working through each batch of training examples\. Valid values: float, range: \(`0.0`, `∞`\)\. Default value: `0.009`\.  | 
| depth |  Depth of the tree\. Valid values: integer, range: \(`1`, `16`\)\. Default value: `6`\.  | 
| l2\_leaf\_reg |  Coefficient for the L2 regularization term of the cost function\. Valid values: integer, range: \(`0`, `∞`\)\. Default value: `3`\.  | 
| random\_strength |  The amount of randomness to use for scoring splits when the tree structure is selected\. Use this parameter to avoid overfitting the model\. Valid values: float, range: \(`0.0`, `∞`\)\. Default value: `1.0`\.  | 