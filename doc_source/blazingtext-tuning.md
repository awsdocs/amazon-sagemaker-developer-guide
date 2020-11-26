# Tune a BlazingText Model<a name="blazingtext-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the BlazingText Algorithm<a name="blazingtext-metrics"></a>

The BlazingText Word2Vec algorithm \(`skipgram`, `cbow`, and `batch_skipgram` modes\) reports on a single metric during training: `train:mean_rho`\. This metric is computed on [WS\-353 word similarity datasets](https://aclweb.org/aclwiki/WordSimilarity-353_Test_Collection_(State_of_the_art))\. When tuning the hyperparameter values for the Word2Vec algorithm, use this metric as the objective\.

The BlazingText Text Classification algorithm \(`supervised` mode\), also reports on a single metric during training: the `validation:accuracy`\. When tuning the hyperparameter values for the text classification algorithm, use these metrics as the objective\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| train:mean\_rho |  The mean rho \(Spearman's rank correlation coefficient\) on [WS\-353 word similarity datasets](http://alfonseca.org/pubs/ws353simrel.tar.gz)  |  Maximize  | 
| validation:accuracy |  The classification accuracy on the user\-specified validation dataset  |  Maximize  | 

## Tunable BlazingText Hyperparameters<a name="blazingtext-tunable-hyperparameters"></a>

### Tunable Hyperparameters for the Word2Vec Algorithm<a name="blazingtext-tunable-hyperparameters-word2vec"></a>

Tune an Amazon SageMaker BlazingText Word2Vec model with the following hyperparameters\. The hyperparameters that have the greatest impact on Word2Vec objective metrics are: `mode`, ` learning_rate`, `window_size`, `vector_dim`, and `negative_samples`\.


| Parameter Name | Parameter Type | Recommended Ranges or Values | 
| --- | --- | --- | 
| batch\_size |  `IntegerParameterRange`  |  \[8\-32\]  | 
| epochs |  `IntegerParameterRange`  |  \[5\-15\]  | 
| learning\_rate |  `ContinuousParameterRange`  |  MinValue: 0\.005, MaxValue: 0\.01  | 
| min\_count |  `IntegerParameterRange`  |  \[0\-100\]  | 
| mode |  `CategoricalParameterRange`  |  \[`'batch_skipgram'`, `'skipgram'`, `'cbow'`\]  | 
| negative\_samples |  `IntegerParameterRange`  |  \[5\-25\]  | 
| sampling\_threshold |  `ContinuousParameterRange`  |  MinValue: 0\.0001, MaxValue: 0\.001  | 
| vector\_dim |  `IntegerParameterRange`  |  \[32\-300\]  | 
| window\_size |  `IntegerParameterRange`  |  \[1\-10\]  | 

### Tunable Hyperparameters for the Text Classification Algorithm<a name="blazingtext-tunable-hyperparameters-text_class"></a>

Tune an Amazon SageMaker BlazingText text classification model with the following hyperparameters\.


| Parameter Name | Parameter Type | Recommended Ranges or Values | 
| --- | --- | --- | 
| buckets |  `IntegerParameterRange`  |  \[1000000\-10000000\]  | 
| epochs |  `IntegerParameterRange`  |  \[5\-15\]  | 
| learning\_rate |  `ContinuousParameterRange`  |  MinValue: 0\.005, MaxValue: 0\.01  | 
| min\_count |  `IntegerParameterRange`  |  \[0\-100\]  | 
| mode |  `CategoricalParameterRange`  |  \[`'supervised'`\]  | 
| vector\_dim |  `IntegerParameterRange`  |  \[32\-300\]  | 
| word\_ngrams |  `IntegerParameterRange`  |  \[1\-3\]  | 