# Tuning a BlazingText Model<a name="blazingtext-tuning"></a>

BlazingText is the Amazon SageMaker implementation of the [Word2Vec](https://en.wikipedia.org/wiki/Word2vec) algorithm that is used to generate word embeddings from a large number of documents\. Word embeddings represent each unique word in the entire collection of text documents as a vector of numbers\. Words that are similar will have similar vectors\. This algorithm is used in a variety of Natural Language Understanding tasks\.

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

For more information about model tuning, see [Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the BlazingText Algorithm<a name="blazingtext-metrics"></a>

The BlazingText algorithm reports on a single metric during training: the `train:mean_rho`\. This metric is computed on WS\-353 word similarity datasets\. When tuning the hyperparameter values, use this metric as the objective\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| train:mean\_rho |  Mean rho \(Spearman's rank correlation coefficient\) on WS\-353 datasets  |  Maximize  | 

## Tunable Hyperparameters<a name="blazingtext-tunable-hyperparameters"></a>

Tune the Amazon SageMaker BlazingText model with the following hyperparameters\. The hyperparameters that have the greatest impact on BlazingText objective metrics are: `mode`, ` learning_rate`, `window_size`, `vector_dim`, and `negative_samples`\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| mode |  CategoricalParameterRange  |  \['batch\_skipgram', 'skipgram', 'cbow'\]  | 
| learning\_rate |  ContinuousParameterRange  |  MinValue: 0\.005, MaxValue: 0\.01  | 
| min\_count |  IntegerParameterRange  |  \[0\-100\]  | 
| window\_size |  IntegerParameterRange  |  \[1\-10\]  | 
| vector\_dim |  IntegerParameterRange  |  \[32\-300\]  | 
| negative\_samples |  IntegerParameterRange  |  \[5\-25\]  | 
| batch\_size |  IntegerParameterRange  |  \[8\-32\]  | 
| sampling\_threshold |  ContinuousParameterRange  |  MinValue: 0\.0001, MaxValue: 0\.001  | 