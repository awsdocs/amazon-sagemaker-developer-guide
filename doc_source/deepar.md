# DeepAR Forecasting<a name="deepar"></a>

Amazon SageMaker DeepAR is a supervised learning algorithm for forecasting scalar \(that is, one\-dimensional\) time series using recurrent neural networks \(RNN\)\. Classical forecasting methods, such as Autoregressive Integrated Moving Average \(ARIMA\) or Exponential Smoothing \(ETS\), fit a single model to each individual time series, and then use that model to extrapolate the time series into the future\. In many applications, however, you encounter many similar time series across a set of cross\-sectional units\. Examples of such time series groupings are demand for different products, server loads, and requests for web pages\. In this case, it can be beneficial to train a single model jointly over all of these time series\. DeepAR takes this approach, outperforming the standard ARIMA and ETS methods when your dataset contains hundreds of related time series\. The trained model can also be used for generating forecasts for new time series that are similar to the ones it has been trained on\.

For the training phase, the dataset consists of one or preferably more than one time series \(`target`\)\. Target time series can be optionally associated with a vector of categorical features \(`cat`\) and a vector of feature time series \(`dynamic_feat`\)\. The DeepAR model is trained by randomly sampling training examples from each target time series in the training dataset\. Each such training example consists of a pair of adjacent context and prediction windows with fixed predefined lengths\. The `context_length` hyperparameter controls how far in the past the network can see, and `prediction_length` how far in the future predictions can be made\. For a more detailed explanation, check the [How DeepAR Works](deepar_how-it-works.md) section\.

## Input/Output Interface<a name="deepar-inputoutput"></a>

DeepAR supports two data channels\. The `train` channel describes the training dataset and is required\. The `test` channel describes a dataset which is used to evaluate a set of accuracy metrics for the model after training and is optional\. You can provide training and test datasets as [JSON Lines](http://jsonlines.org/), which can be gzipped, or [Parquet](https://parquet.apache.org/) files\.

The training and the test paths can point to a single file or a directory containing multiple files \(possibly nested in subdirectories\)\. If the path is a directory, DeepAR considers all files in the directory, except files starting with a dot and files named *\_SUCCESS*, as inputs for the corresponding channel\. This convention ensures that you can directly use output folders produced by Spark jobs as input channels for your DeepAR training jobs\.

By default, the DeepAR model determines the input format from the file extension \(either `.json` or `.json.gz` or `.parquet`\) of the specified input path\. If the path does not end in one of these suffixes, you must specify the format explicitly\. In the Python SDK, this is achieved using the `content_type` parameter of the [s3\_input](https://sagemaker.readthedocs.io/en/stable/session.html#sagemaker.session.s3_input) class\.

The records in your input files should contain the following fields:
+ `start`: a string of the format `YYYY-MM-DD HH:MM:SS`\. The start time stamp should not contain any time zone information\.
+ `target`: an array of floats \(or integers\) that represent the time series\. Missing values can be encoded as `null` literals or `"NaN"` strings \(in JSON\), or as `nan` float values \(in Parquet\)\.
+ `dynamic_feat` \(optional\): an array of arrays of floats \(or integers\) that represents the vector of custom feature time series \(dynamic features\)\. If the field is set, all records must have the same number of inner arrays \(that is, the same number of feature time series\)\. In addition, each inner array must have the same length as the associated `target` value\. Missing values are not supported in the features\. 
+ `cat` \(optional\): an array of categorical features that can be used to encode groups to which the record belongs\. Categorical features must be encoded as a 0\-based sequence of positive integers\. For example, the categorical domain \{R, G, B\} can be encoded as \{0, 1, 2\}\. Clients must ensure that all values from each categorical domain are represented in the training dataset\. This restriction is necessary because during inference we can only forecast for categories which have been observed during training\. Further, each categorical feature is embedded in a low dimensional space whose dimensionality is controlled by the `embedding_dimension` hyperparameter \(see [DeepAR Hyperparameters](deepar_hyperparameters.md)\)\.

If you use a JSON file, it must be in the [JSON Lines](http://jsonlines.org/) format\. The following is an example of JSON data:

```
{"start": "2009-11-01 00:00:00", "target": [4.3, "NaN", 5.1, ...], "cat": [0, 1], "dynamic_feat": [[1.1, 1.2, 0.5, ..]]}
{"start": "2012-01-30 00:00:00", "target": [1.0, -5.0, ...], "cat": [2, 3], "dynamic_feat": [[1.1, 2.05, ...]]}
{"start": "1999-01-30 00:00:00", "target": [2.0, 1.0], "cat": [1, 4], "dynamic_feat": [[1.3, 0.4]]}
```

In this example, each time series has two categorical features and one time series features associated\.

For Parquet, you use the same three fields as columns\. In addition, `"start"` can be the `datetime` type\. Parquet files can be compressed using `gzip` and `snappy` compression\.

For training data:
+ Different time series may differ in their start time and their length but all series must have the same frequency, the same number of categorical features and the same number of dynamic features\. 
+ The training file should be shuffled with respect to the position of the time series in the file\. In other words, the time series should occur in a random order in the file\.
+ The `start` time stamp is used for deriving the internal features\. So it is important that the `start` field is set correctly\.
+ If you use categorical features \(`cat`\), all time series must have the same number of categorical features\. If the dataset contains the `cat` field, it is used and the cardinality of the groups is extracted from the dataset \(`cardinality = "auto"` is default\)\. If the dataset contains the `cat` field, but you do not want to use, you can disable the use of the categorical feature by setting \(`cardinality = ""`\)\. If a model was trained using a `cat` feature, this feature has to be provided for prediction\.
+ If your dataset contains the field `dynamic_feat`, it is used automatically\. All time series have to have the same number of feature time series\. The time points in each of the feature time series correspond one\-to\-one to the time points in the target and entry in `dynamic_feat` should have the same length as the `target`\. If the dataset contains the `dynamic_feat` field, but you do not want to use it, you can disable the use by setting \(`num_dynamic_feat = ""`\)\. If the model was trained with the dynamic\_feat field this field has to be provided for inference and each of the features has to have the length of the provided target \+ `prediction_length`\. \(i\.e\., the feature value in the future has to be provided\)\.

If you specify optional test channel data, the DeepAR algorithm evaluates the trained model with different accuracy metrics\. The algorithm calculates the root mean square error \(RMSE\) over the test data as follows:

![\[RMSE Formula: Sqrt(1/nT(Sum[i,t](y-hat(i,t)-y(i,t))^2))\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/deepar-1.png)

where *y**i*,*t* is the true value of time series *i* at time *t* and *ŷ**i*,*t* is the mean prediction\. The sum is over all *n* time series in the test set and over the last Τ time points for each time series, where Τ corresponds to the forecast horizon\. You specify the length of the forecast horizon by setting the `prediction_length` hyperparameter \(see [DeepAR Hyperparameters](deepar_hyperparameters.md)\)\.

In addition, the accuracy of the forecast distribution is evaluated using weighted quantile loss\. For a quantile in the range \[0, 1\], the weighted quantile loss is defined as follows:

![\[Quantile loss\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/deepar-2.png)

Here, *q**i*,*t*\(τ\) is the τ\-quantile of the distribution that the model predicts\. Set the *test\_quantiles* hyperparameter to specify which quantiles for which the algorithm calculates quantile loss\. In addition to these, the average of the prescribed quantile losses is reported as part of the training logs\. For information, see [DeepAR Hyperparameters](deepar_hyperparameters.md)\. 

For inference, DeepAR accepts JSON format with an `"instances"` field which includes one or more time series in JSON Lines format, and a name of `"configuration"`, which includes parameters for generating the forecast\. For details, see [DeepAR Inference Formats](deepar-in-formats.md)\.

## Recommended Best Practices<a name="deepar_best_practices"></a>

To achieve optimal result, the following points should be considered when preparing the time series data:
+ Except for the purpose of the train/test split discussed below, always provide entire time series for training, testing, and when calling the model for prediction\. Do not cut the time series up or provide only a part of the time series irrespective of how you set `context_length`\. The model uses data points further back than `context_length` for the lagged values feature\.
+ For [Tuning a DeepAR Model](deepar-tuning.md), you can split the dataset into a training and a test dataset\. In a typical evaluation scenario, you would like to test the model on the same time series used in the training, but on the future `prediction_length` time points following immediately after the last time point visible during training\. A simple way to create train and test sets satisfying these criteria is to use the entire dataset \(that is, th full length of all time series available\) as a test set and remove the last `prediction_length` points from each time series for training\. This way, during training, the model does not see the target values for time points on which it is evaluated during testing\. In the test phase, the last `prediction_length` points of each time series in the test set are withheld and a prediction is generated\. The forecast is then compared with the withheld values\. You can create more complex evaluations by repeating time series multiple times in the test set, but cutting them at different end points\. This results in accuracy metrics averaged over multiple forecasts from different time points\.
+ Try not to use very large values \(> 400\) for the `prediction_length`, since this makes the model slow and less accurate\. If you want to forecast further into the future, consider aggregating to a higher frequency\. For example, use `5min` instead of `1min`\.
+ Due to the use of lags, a model can look further back in the time series than `context_length` and so it is not necessary to set this parameter to a large value\. A good starting value for this parameter is to use the same value as used for the `prediction_length`\.
+ We recommend training DeepAR model on as many time series as available\. While a DeepAR model trained on a single time series may already work well, standard forecasting methods such as ARIMA or ETS may be more accurate and are more tailored to this use case\. The DeepAR approach really starts to outperform the standard methods when your dataset contains hundreds of related time series\.

## EC2 Instance Recommendations<a name="deepar-instances"></a>

You can train DeepAR on both GPU and CPU instances, in both single and multi\-machine settings\. We recommend starting with a single CPU instance \(for example, *ml\.c4\.xlarge* or *ml\.c4\.2xlarge*\), and switching to GPU instances and multiple machines only when necessary\. Using GPUs and multiple machines improves throughput only for larger models \(many cells per layer, many layers\) and/or a large mini\-batch size \(e\.g\. greater than 512\)

For inference, DeepAR only supports CPU instances\.

Large values of `context_length`, `prediction_length`, `num_cells`, `num_layers`, `mini_batch_size`, can lead to large model sizes that may not fit into small instances\. In this case, use a larger machine or reduce the values for these parameters\. This problem also frequently occurs when running hyperparameter tuning jobs\. In that case, use an instance type large enough for the model tuning job and consider limiting the upper values of the critical parameters to avoid job failures\. 

**Topics**
+ [Input/Output Interface](#deepar-inputoutput)
+ [Recommended Best Practices](#deepar_best_practices)
+ [EC2 Instance Recommendations](#deepar-instances)
+ [How DeepAR Works](deepar_how-it-works.md)
+ [DeepAR Hyperparameters](deepar_hyperparameters.md)
+ [Tuning a DeepAR Model](deepar-tuning.md)
+ [DeepAR Inference Formats](deepar-in-formats.md)