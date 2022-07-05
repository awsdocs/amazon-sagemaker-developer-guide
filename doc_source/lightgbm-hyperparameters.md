# LightGBM hyperparameters<a name="lightgbm-hyperparameters"></a>

The following table contains the subset of hyperparameters that are required or most commonly used for the Amazon SageMaker LightGBM algorithm\. Users set these parameters to facilitate the estimation of model parameters from data\. The SageMaker LightGBM algorithm is an implementation of the open\-source [LightGBM](https://github.com/microsoft/LightGBM) package\.

The SageMaker LightGBM algorithm automatically chooses an evaluation metric and objective function based on the type of classification problem\. The LightGBM algorithm detects the type of classification problem based on the number of labels in your data\. For regression problems, the evaluation metric is root mean squared error and the objective function is L2 loss\. For binary classification problems, the evaluation metric and objective function are both binary cross entropy\. For multiclass classification problems, the evaluation metric is multiclass cross entropy and the objective function is softmax\.

**Note**  
The LightGBM evaluation metric and objective functions are not currently available as hyperparameters\. Instead, the Amazon SageMaker LightGBM built\-in algorithm automatically detects the type of classification task \(regression, binary, or multiclass\) based on the number of unique integers in the label column and assigns an evaluation metric and objective function\.


| Parameter Name | Description | 
| --- | --- | 
| num\_boost\_round |  The maximum number of boosting iterations\. **Note:** Internally, LightGBM constructs `num_class * num_boost_round` trees for multi\-class classification problems\. Valid values: integer, range: \(`0`, `∞`\)\. Default value: `5000`\.  | 
| early\_stopping\_rounds |  The training will stop if one metric of one validation data point does not improve in the last `early_stopping_rounds` round\. If `early_stopping_rounds` is less than or equal to zero, this hyperparameter is ignored\. Valid values: integer\. Default value: `30`\.  | 
| learning\_rate |  The rate at which the model weights are updated after working through each batch of training examples\. Valid values: float, range: \(`0.0`, `∞`\)\. Default value: `0.009`\.  | 
| num\_leaves |  The maximum number of leaves in one tree\. Valid values: integer, range: \(`1`, `131072`\)\. Default value: `67`\.  | 
| feature\_fraction |  A subset of features to be selected on each iteration \(tree\)\. Must be less than 1\.0\. Valid values: float, range: \(`0.0`, `1.0`\)\. Default value: `0.74`\.  | 
| bagging\_fraction |  A subset of features similar to `feature_fraction`, but `bagging_fraction` randomly selects part of the data without resampling\. Valid values: float, range: \(`0.0`, `1.0`\)\. Default value: `0.53`\.  | 
| bagging\_freq |  The frequency to perform bagging\. At every `bagging_freq` iteration, LightGBM randomly selects a percentage of the data to use for the next `bagging_freq` iteration\. This percentage is determined by the `bagging_fraction` hyperparameter\. If `bagging_freq` is zero, then bagging is deactivated\. Valid values: integer, range: \(`0`, `∞`\)\. Default value: `5`\.  | 
| max\_depth |  The maximum depth for a tree model\. This is used to deal with overfitting when the amount of data is small\. If `max_depth` is less than or equal to zero, this means there is no limit for maximum depth\. Valid values: integer\. Default value: `11`\.  | 
| min\_data\_in\_leaf |  The minimum amount of data in one leaf\. Can be used to deal with overfitting\. Valid values: integer, range: \(`0`, `∞`\)\. Default value: `0.25`\.  | 