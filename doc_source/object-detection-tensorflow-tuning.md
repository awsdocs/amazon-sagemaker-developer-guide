# Tune an Object Detection \- TensorFlow model<a name="object-detection-tensorflow-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

For more information about model tuning, see [Perform Automatic Model Tuning with SageMaker](automatic-model-tuning.md)\.

## Metrics computed by the Object Detection \- TensorFlow algorithm<a name="object-detection-tensorflow-metrics"></a>

Refer to the following chart to find which metrics are computed by the Object Detection \- TensorFlow algorithm\.


| Metric Name | Description | Optimization Direction | Regex Pattern | 
| --- | --- | --- | --- | 
| validation:localization\_loss | The localization loss for box prediction\. | Minimize | `Val_localization=([0-9\\.]+)` | 

## Tunable Object Detection \- TensorFlow hyperparameters<a name="object-detection-tensorflow-tunable-hyperparameters"></a>

Tune an object detection model with the following hyperparameters\. The hyperparameters that have the greatest impact on object detection objective metrics are: `batch_size`, `learning_rate`, and `optimizer`\. Tune the optimizer\-related hyperparameters, such as `momentum`, `regularizers_l2`, `beta_1`, `beta_2`, and `eps` based on the selected `optimizer`\. For example, use `beta_1` and `beta_2` only when `adam` is the `optimizer`\.

For more information about which hyperparameters are used for each `optimizer`, see [Object Detection \- TensorFlow Hyperparameters](object-detection-tensorflow-Hyperparameter.md)\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| batch\_size | IntegerParameterRanges | MinValue: 8, MaxValue: 512 | 
| beta\_1 | ContinuousParameterRanges | MinValue: 1e\-6, MaxValue: 0\.999 | 
| beta\_2 | ContinuousParameterRanges | MinValue: 1e\-6, MaxValue: 0\.999 | 
| eps | ContinuousParameterRanges | MinValue: 1e\-8, MaxValue: 1\.0 | 
| learning\_rate | ContinuousParameterRanges | MinValue: 1e\-6, MaxValue: 0\.5 | 
| momentum | ContinuousParameterRanges | MinValue: 0\.0, MaxValue: 0\.999 | 
| optimizer | CategoricalParameterRanges | \['sgd', ‘adam’, ‘rmsprop’, 'nesterov', 'adagrad', 'adadelta'\] | 
| regularizers\_l2 | ContinuousParameterRanges | MinValue: 0\.0, MaxValue: 0\.999 | 
| train\_only\_on\_top\_layer | CategoricalParameterRanges | \['True', 'False'\] | 
| initial\_accumulator\_value | CategoricalParameterRanges | MinValue: 0\.0, MaxValue: 0\.999 | 