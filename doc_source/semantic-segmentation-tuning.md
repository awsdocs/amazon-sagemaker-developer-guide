# Tuning a Semantic Segmentation Model<a name="semantic-segmentation-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

## Metrics Computed by the Semantic Segmentation Algorithm<a name="semantic-segmentation-metrics"></a>

The semantic segmentation algorithm reports two validation metrics\. When tuning hyperparameter values, choose one of these metrics as the objective\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| validation:mIOU |  The area of the intersection of the predicted segmentation and the ground truth divided by the area of union between them for images in the validation set\. Also known as the Jaccard Index\.  |  Maximize  | 
| validation:pixel\_accuracy | The percentage of pixels that are correctly classified in images from the validation set\. |  Maximize  | 

## Tunable Semantic Segmentation Hyperparameters<a name="semantic-segmentation-tunable-hyperparameters"></a>

You can tune the following hyperparameters for the semantic segmentation algorithm\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| learning\_rate |  ContinuousParameterRange  |  MinValue: 1e\-4, MaxValue: 1e\-1  | 
| mini\_batch\_size |  IntegerParameterRanges  |  MinValue: 1, MaxValue: 128  | 
| momentum |  ContinuousParameterRange  |  MinValue: 0\.9, MaxValue: 0\.999  | 
| optimzer |  CategoricalParameterRanges  |  \['sgd', 'adam', 'rmsprop', 'adagrad', 'nag'\]  | 
| weight\_decay |  ContinuousParameterRange  |  MinValue: 1e\-5, MaxValue: 1e\-3  | 
