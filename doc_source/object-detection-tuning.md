# Tune an Object Detection Model<a name="object-detection-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the Object Detection Algorithm<a name="object-detection-metrics"></a>

The object detection algorithm reports on a single metric during training: `validation:mAP`\. When tuning a model, choose this metric as the objective metric\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| validation:mAP |  Mean Average Precision \(mAP\) computed on the validation set\.  |  Maximize  | 

## Tunable Object Detection Hyperparameters<a name="object-detection-tunable-hyperparameters"></a>

Tune the Amazon SageMaker object detection model with the following hyperparameters\. The hyperparameters that have the greatest impact on the object detection objective metric are: `mini_batch_size`, `learning_rate`, and `optimizer`\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| learning\_rate |  ContinuousParameterRange  |  MinValue: 1e\-6, MaxValue: 0\.5  | 
| mini\_batch\_size |  IntegerParameterRanges  |  MinValue: 8, MaxValue: 64  | 
| momentum |  ContinuousParameterRange  |  MinValue: 0\.0, MaxValue: 0\.999  | 
| optimizer |  CategoricalParameterRanges  |  \['sgd', 'adam', 'rmsprop', 'adadelta'\]  | 
| weight\_decay |  ContinuousParameterRange  |  MinValue: 0\.0, MaxValue: 0\.999  | 