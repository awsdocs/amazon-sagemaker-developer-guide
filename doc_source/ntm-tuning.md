# Tune an NTM Model<a name="ntm-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

Amazon SageMaker NTM is an unsupervised learning algorithm that learns latent representations of large collections of discrete data, such as a corpus of documents\. Latent representations use inferred variables that are not directly measured to model the observations in a dataset\. Automatic model tuning on NTM helps you find the model that minimizes loss over the training or validation data\. *Training loss* measures how well the model fits the training data\. *Validation loss* measures how well the model can generalize to data that it is not trained on\. Low training loss indicates that a model is a good fit to the training data\. Low validation loss indicates that a model has not overfit the training data and so should be able to model documents successfully on which is has not been trained\. Usually, it's preferable to have both losses be small\. However, minimizing training loss too much might result in overfitting and increase validation loss, which would reduce the generality of the model\. 

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the NTM Algorithm<a name="ntm-metrics"></a>

The NTM algorithm reports a single metric that is computed during training: `validation:total_loss`\. The total loss is the sum of the reconstruction loss and Kullback\-Leibler divergence\. When tuning hyperparameter values, choose this metric as the objective\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| validation:total\_loss |  Total Loss on validation set  |  Minimize  | 

## Tunable NTM Hyperparameters<a name="ntm-tunable-hyperparameters"></a>

You can tune the following hyperparameters for the NTM algorithm\. Usually setting low `mini_batch_size` and small `learning_rate` values results in lower validation losses, although it might take longer to train\. Low validation losses don't necessarily produce more coherent topics as interpreted by humans\. The effect of other hyperparameters on training and validation loss can vary from dataset to dataset\. To see which values are compatible, see [NTM Hyperparameters](ntm_hyperparameters.md)\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| encoder\_layers\_activation |  CategoricalParameterRanges  |  \['sigmoid', 'tanh', 'relu'\]  | 
| learning\_rate |  ContinuousParameterRange  |  MinValue: 1e\-4, MaxValue: 0\.1  | 
| mini\_batch\_size |  IntegerParameterRanges  |  MinValue: 16, MaxValue:2048  | 
| optimizer |  CategoricalParameterRanges  |  \['sgd', 'adam', 'adadelta'\]  | 
| rescale\_gradient |  ContinuousParameterRange  |  MinValue: 0\.1, MaxValue: 1\.0  | 
| weight\_decay |  ContinuousParameterRange  |  MinValue: 0\.0, MaxValue: 1\.0  | 