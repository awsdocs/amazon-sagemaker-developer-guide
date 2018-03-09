# DeepAR Forecasting<a name="deepar"></a>

Amazon SageMaker DeepAR is a supervised learning algorithm for forecasting scalar time series using recurrent neural networks \(RNN\)\. Classical forecasting methods, such as Autoregressive Integrated Moving Average \(ARIMA\) or Exponential Smoothing \(ETS\), fit one model to each individual time series, and then use that model to extrapolate the time series into the future\. In many applications, however, you might have many similar time series across a set of cross\-sectional units \(for example, demand for different products, load of servers, requests for web pages, and so on\)\. In this case, it can be beneficial to train a single model jointly over all of these time series\. DeepAR takes this approach, training a model for predicting a time series over a large set of \(related\) time series\.

For the training phase, the dataset consists of one or more time series, and, optionally a categorical grouping variable that the time series is a member of\. The model learns entirely from these values\. The DeepAR algorithm accepts no other external features \. The model is then trained by randomly selecting time points from the provided time series and using them as training examples\.

For inference, the trained model takes as input an individual time series, which might or might not have been used during training, and generates a forecast for the time series\. This forecast takes into account what typically happened for similar time series in the training set\. 

## Input/Output Interface<a name="deepar-inputoutput"></a>

DeepAR supports two data channels\. The train channel is used for training a model and is required\. The test channel is optional\. If the test channel is present, the algorithm uses it to calculate accuracy metrics for the model after training\. You can provide datasets as JSON or [Parquet](https://parquet.apache.org/) files\.

By default, the model determines the input format from the file extension \(either `.json` or `.parquet`\. If you provide input files with different extensions, you can specify the file type by setting the `ContentType` parameter of the [Channel](API_Channel.md) data type\. 

If you use a JSON file, it must be in the [JSON Lines](http://jsonlines.org/) format, where each record contains the following fields:

+ `“start”` whose value is a string of the format `YYYY-MM-DD HH:MM:SS`\.

+ `“target”`, whose value is an array of floats \(or integers\) that represent the time series variable’s values\.

+ `“cat”` \(optional\), whose value is an integer that encodes the categorical grouping that record’s time series is a member of\. The categorical feature allows the model to learn typical behavior for that group\. This can increase accuracy\.

The following is an example of JSON data:

```
{"start":"2009-11-01 00:00:00", "target": [4.3, 10.3, ...], "cat": 0}
{"start":"2012-01-30 00:00:00", "target": [1.0, -5.0, ...], "cat": 2}
{"start":"1999-01-30 00:00:00", "target": [2.0, 1.0], "cat": 0}
```

For Parquet, you use the same three fields as columns\. In addition, `“start”` can be the `datetime` type\. `gzip` and `snappy` compression types are also supported\.

For training data:

+ All time series must have the same time unit: minutes, hours, days, weeks, or months\.

+ To train an accurate model, the training set should contain a sufficient number of time series \(typically at least a few hundred\) and should cover a representative time range\. For example, one or more years when yearly seasonal patterns occur\.\.

+ The training file should be shuffled\. In other words, the time series should occur in a random order in the file\.

+ If you use the categorical feature \(`"cat"`\), all time series must have this feature\.

If you specify optional test channel data, the DeepAR algorithm evaluates the trained model with different accuracy metrics\. The algorithm calculates the root mean square error \(RMSE\) over the test data as follows:

![\[Squrt(1/nT(Sum[i,t](y-hat(i,t)-y(i,t))^2))\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/deepar-1.png)

where *y**i*,*t* is the true value of time series *i* at time *t* and *ŷ**i*,*t* is the mean prediction\. The sum is over all *n* time series in the test set and over the last Τ time points for each time series, where Τ corresponds to the forecast horizon\. You specify the forecast horizon by setting the `prediction_length` hyperparameter \(see [DeepAR Hyperparameters](deepar_hyperparameters.md)\)\.

In addition, the accuracy of the forecast distribution is evaluated using weighted quantile loss\. For a quantile in the range \[0, 1\], the weighted quantile loss is defined as follows:

![\[quantile loss\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/deepar-2.png)

Here, *q**i*,*t*\(τ\) is the τ\-quantile of the distribution that the model predicts\. Set the *test\_quantiles* hyperparameter to specify which quantiles for which the algorithm calculates quantile loss\. For information, see [DeepAR Hyperparameters](deepar_hyperparameters.md)\. 

If you have a set of time series, as simple way to prepare, test, and train datasets is as follows:

+ In the train channel, remove the last `prediction_length` points from each time series\.

+ Use the full dataset in the test channel\.

This ensures that the model does not see the removed points during training, and then those points are used for calculating the accuracy of the model\.

For inference, DeepAR accepts JSON format with an `“instances”` field which includes one or more time series in JSON Lines format, and a name of `“configuration”`, which includes parameters for generating the forecast\. For details, see [DeepAR Request and Response Formats](deepar-in-formats.md)\.

## DeepAR Instance Recommendations<a name="deepar-instances"></a>

You can train DeepAR on both GPU and CPU instances, in both single and multi\-machine settings\. We recommend starting with a single CPU instance \(for example, c4\.xlarge or c4\.2xlarge\), and switching to GPU instances and multiple machines only when necessary\. Using GPUs and multiple machines improves performance only when the model has more than 100 cells in each hidden layer and/or a mini\-batch size greater than 1000\.

For information on the mathematics behind DeepAR, see [DeepAR: Probabilistic Forecasting with Autoregressive Recurrent Networks](https://arxiv.org/abs/1704.04110)\. 


+ [Input/Output Interface](#deepar-inputoutput)
+ [DeepAR Instance Recommendations](#deepar-instances)
+ [DeepAR Hyperparameters](deepar_hyperparameters.md)
+ [DeepAR Request and Response Formats](deepar-in-formats.md)