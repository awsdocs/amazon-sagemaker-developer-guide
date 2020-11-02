# Tune a Sequence\-to\-Sequence Model<a name="seq-2-seq-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the Sequence\-to\-Sequence Algorithm<a name="seq-2-seq-metrics"></a>

The sequence to sequence algorithm reports three metrics that are computed during training\. Choose one of them as an objective to optimize when tuning the hyperparameter values\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| validation:accuracy |  Accuracy computed on the validation dataset\.  |  Maximize  | 
| validation:bleu |  [Bleu﻿](https://en.wikipedia.org/wiki/BLEU) score computed on the validation dataset\. Because BLEU computation is expensive, you can choose to compute BLEU on a random subsample of the validation dataset to speed up the overall training process\. Use the `bleu_sample_size` parameter to specify the subsample\.  |  Maximize  | 
| validation:perplexity |  [Perplexity](https://en.wikipedia.org/wiki/Perplexity), is a loss function computed on the validation dataset\. Perplexity measures the cross\-entropy between an empirical sample and the distribution predicted by a model and so provides a measure of how well a model predicts the sample values, Models that are good at predicting a sample have a low perplexity\.  |  Minimize  | 

## Tunable Sequence\-to\-Sequence Hyperparameters<a name="seq-2-seq-tunable-hyperparameters"></a>

You can tune the following hyperparameters for the SageMaker Sequence to Sequence algorithm\. The hyperparameters that have the greatest impact on sequence to sequence objective metrics are: `batch_size`, `optimizer_type`, `learning_rate`, `num_layers_encoder`, and `num_layers_decoder`\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| num\_layers\_encoder |  IntegerParameterRange  |  \[1\-10\]  | 
| num\_layers\_decoder |  IntegerParameterRange  |  \[1\-10\]  | 
| batch\_size |  CategoricalParameterRange  |  \[16,32,64,128,256,512,1024,2048\]  | 
| optimizer\_type |  CategoricalParameterRange  |  \['adam', 'sgd', 'rmsprop'\]  | 
| weight\_init\_type |  CategoricalParameterRange  |  \['xavier', 'uniform'\]  | 
| weight\_init\_scale |  ContinuousParameterRange  |  For the xavier type: MinValue: 2\.0, MaxValue: 3\.0 For the uniform type: MinValue: \-1\.0, MaxValue: 1\.0  | 
| learning\_rate |  ContinuousParameterRange  |  MinValue: 0\.00005, MaxValue: 0\.2  | 
| weight\_decay |  ContinuousParameterRange  |  MinValue: 0\.0, MaxValue: 0\.1  | 
| momentum |  ContinuousParameterRange  |  MinValue: 0\.5, MaxValue: 0\.9  | 
| clip\_gradient |  ContinuousParameterRange  |  MinValue: 1\.0, MaxValue: 5\.0  | 
| rnn\_num\_hidden |  CategoricalParameterRange  |  Applicable only to recurrent neural networks \(RNNs\)\. \[128,256,512,1024,2048\]   | 
| cnn\_num\_hidden |  CategoricalParameterRange  |  Applicable only to convolutional neural networks \(CNNs\)\. \[128,256,512,1024,2048\]   | 
| num\_embed\_source |  IntegerParameterRange  |  \[256\-512\]  | 
| num\_embed\_target |  IntegerParameterRange  |  \[256\-512\]  | 
| embed\_dropout\_source |  ContinuousParameterRange  |  MinValue: 0\.0, MaxValue: 0\.5  | 
| embed\_dropout\_target |  ContinuousParameterRange  |  MinValue: 0\.0, MaxValue: 0\.5  | 
| rnn\_decoder\_hidden\_dropout |  ContinuousParameterRange  |  MinValue: 0\.0, MaxValue: 0\.5  | 
| cnn\_hidden\_dropout |  ContinuousParameterRange  |  MinValue: 0\.0, MaxValue: 0\.5  | 
| lr\_scheduler\_type |  CategoricalParameterRange  |  \['plateau\_reduce', 'fixed\_rate\_inv\_t', 'fixed\_rate\_inv\_sqrt\_t'\]  | 
| plateau\_reduce\_lr\_factor |  ContinuousParameterRange  |  MinValue: 0\.1, MaxValue: 0\.5  | 
| plateau\_reduce\_lr\_threshold |  IntegerParameterRange  |  \[1\-5\]  | 
| fixed\_rate\_lr\_half\_life |  IntegerParameterRange  |  \[10\-30\]  | 