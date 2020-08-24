# Tune a linear learner model<a name="linear-learner-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\. 

The linear learner algorithm also has an internal mechanism for tuning hyperparameters separate from the automatic model tuning feature described here\. By default, the linear learner algorithm tunes hyperparameters by training multiple models in parallel\. When you use automatic model tuning, the linear learner internal tuning mechanism is turned off automatically\. This sets the number of parallel models, `num_models`, to 1\. The algorithm ignores any value that you set for `num_models`\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics computed by the linear learner algorithm<a name="linear-learner-metrics"></a>

The linear learner algorithm reports the metrics in the following table, which are computed during training\. Choose one of them as the objective metric\. To avoid overfitting, we recommend tuning the model against a validation metric instead of a training metric\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:objective\_loss |  The mean value of the objective loss function on the test dataset after the model is trained\. By default, the loss is logistic loss for binary classification and squared loss for regression\. To set the loss to other types, use the `loss` hyperparameter\.  |  Minimize  | 
| test:binary\_classification\_ accuracy |  The accuracy of the final model on the test dataset\.  |  Maximize  | 
| test:binary\_f\_beta |  The F\_beta score of the final model on the test dataset\. By default, it is the F1 score, which is the harmonic mean of precision and recall\.  |  Maximize  | 
| test:precision |  The precision of the final model on the test dataset\. If you choose this metric as the objective, we recommend setting a target recall by setting the `binary_classifier_model_selection` hyperparameter to `precision_at_target_recall` and setting the value for the `target_recall` hyperparameter\.  |  Maximize  | 
| test:recall |  The recall of the final model on the test dataset\. If you choose this metric as the objective, we recommend setting a target precision by setting the `binary_classifier_model_selection` hyperparameter to `recall_at_target_precision` and setting the value for the `target_precision` hyperparameter\.  |  Maximize  | 
| validation:objective\_loss |  The mean value of the objective loss function on the validation dataset every epoch\. By default, the loss is logistic loss for binary classification and squared loss for regression\. To set loss to other types, use the `loss` hyperparameter\.  |  Minimize  | 
| validation:binary\_classification\_accuracy |  The accuracy of the final model on the validation dataset\.  |  Maximize  | 
| validation:binary\_f\_beta |  The F\_beta score of the final model on the validation dataset\. By default, the F\_beta score is the F1 score, which is the harmonic mean of the `validation:precision` and `validation:recall` metrics\.  |  Maximize  | 
| validation:precision |  The precision of the final model on the validation dataset\. If you choose this metric as the objective, we recommend setting a target recall by setting the `binary_classifier_model_selection` hyperparameter to `precision_at_target_recall` and setting the value for the `target_recall` hyperparameter\.  |  Maximize  | 
| validation:recall |  The recall of the final model on the validation dataset\. If you choose this metric as the objective, we recommend setting a target precision by setting the `binary_classifier_model_selection` hyperparameter to `recall_at_target_precision` and setting the value for the `target_precision` hyperparameter\.  |  Maximize  | 

## Tuning linear learner hyperparameters<a name="linear-learner-tunable-hyperparameters"></a>

You can tune a linear learner model with the following hyperparameters\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| wd |  `ContinuousParameterRanges`  |  `MinValue: ``1e-7`, `MaxValue`: `1`  | 
| l1 |  `ContinuousParameterRanges`  |  `MinValue`: `1e-7`, `MaxValue`: `1`  | 
| learning\_rate |  `ContinuousParameterRanges`  |  `MinValue`: `1e-5`, `MaxValue`: `1`  | 
| mini\_batch\_size |  `IntegerParameterRanges`  |  `MinValue`: `100`, `MaxValue`: `5000`  | 
| use\_bias |  `CategoricalParameterRanges`  |  `[True, False]`  | 
| positive\_example\_weight\_mult |  `ContinuousParameterRanges`  |  `MinValue`: 1e\-5, `MaxValue`: `1e5`  | 