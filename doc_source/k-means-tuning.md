# Tune a K\-Means Model<a name="k-means-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

The Amazon SageMaker k\-means algorithm is an unsupervised algorithm that groups data into clusters whose members are as similar as possible\. Because it is unsupervised, it doesn't use a validation dataset that hyperparameters can optimize against\. But it does take a test dataset and emits metrics that depend on the squared distance between the data points and the final cluster centroids at the end of each training run\. To find the model that reports the tightest clusters on the test dataset, you can use a hyperparameter tuning job\. The clusters optimize the similarity of their members\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the K\-Means Algorithm<a name="km-metrics"></a>

The k\-means algorithm computes the following metrics during training\. When tuning a model, choose one of these metrics as the objective metric\. 


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:msd | Mean squared distances between each record in the test set and the closest center of the model\. | Minimize | 
| test:ssd | Sum of the squared distances between each record in the test set and the closest center of the model\. | Minimize | 

## Tunable K\-Means Hyperparameters<a name="km-tunable-hyperparameters"></a>

Tune the Amazon SageMaker k\-means model with the following hyperparameters\. The hyperparameters that have the greatest impact on k\-means objective metrics are: `mini_batch_size`, `extra_center_factor`, and `init_method`\. Tuning the hyperparameter `epochs` generally results in minor improvements\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| epochs | IntegerParameterRanges | MinValue: 1, MaxValue:10 | 
| extra\_center\_factor | IntegerParameterRanges | MinValue: 4, MaxValue:10 | 
| init\_method | CategoricalParameterRanges | \['kmeans\+\+', 'random'\] | 
| mini\_batch\_size | IntegerParameterRanges | MinValue: 3000, MaxValue:15000 | 