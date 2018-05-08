# DeepAR Forecasting<a name="deepar"></a>

Amazon SageMaker DeepAR is a supervised learning algorithm for forecasting scalar time series using recurrent neural networks \(RNN\)\. Classical forecasting methods, such as Autoregressive Integrated Moving Average \(ARIMA\) or Exponential Smoothing \(ETS\), fit one model to each individual time series, and then use that model to extrapolate the time series into the future\. In many applications, however, you might have many similar time series across a set of cross\-sectional units \(for example, demand for different products, load of servers, requests for web pages, and so on\)\. In this case, it can be beneficial to train a single model jointly over all of these time series\. DeepAR takes this approach, training a model for predicting a time series over a large set of \(related\) time series\.

For the training phase, the dataset consists of one or preferably more than one time series, and an optional categorical grouping variable of which the time series is a member\. The model learns entirely from these values\. The DeepAR algorithm currently accepts no other external features\. The model is then trained by randomly selecting time points from the provided time series and using them as training examples\.

For inference, the trained model takes as input an individual time series, which might or might not have been used during training, and generates a forecast for the time series\. This forecast takes into account what typically happened for similar time series in the training set\. 

## Input/Output Interface<a name="deepar-inputoutput"></a>

DeepAR supports two data channels\. The train channel is used for training a model and is required\. The test channel is optional\. If the test channel is present, the algorithm uses it to calculate accuracy metrics for the model after training\. You can provide datasets as JSON or [Parquet](https://parquet.apache.org/) files\.

By default, the model determines the input format from the file extension \(either `.json` or `.parquet`\. If you provide input files with different extensions, you can specify the file type by setting the `ContentType` parameter of the [Channel](API_Channel.md) data type\. 

If you use a JSON file, it must be in the [JSON Lines](http://jsonlines.org/) format, where each record contains the following fields:
+ `"start"` whose value is a string of the format `YYYY-MM-DD HH:MM:SS`\.
+ `"target"`, whose value is an array of floats \(or integers\) that represent the time series variable’s values\.
+ `"cat"` \(optional\), whose value is an integer that encodes the categorical grouping that record’s time series is a member of\. The categorical feature allows the model to learn typical behavior for that group\. This can increase accuracy\.

The following is an example of JSON data:

```
{"start":"2009-11-01 00:00:00", "target": [4.3, 10.3, ...], "cat": 0}
{"start":"2012-01-30 00:00:00", "target": [1.0, -5.0, ...], "cat": 2}
{"start":"1999-01-30 00:00:00", "target": [2.0, 1.0], "cat": 1}
```

For Parquet, you use the same three fields as columns\. In addition, `"start"` can be the `datetime` type\. `gzip` and `snappy` compression types are also supported\.

For training data:
+ All time series must have the same time unit: minutes, hours, days, weeks, or months\.
+ To train an accurate model, the training set should contain a sufficient number of time series \(typically at least a few hundred\) and should cover a representative time range\. For example, one or more years when yearly seasonal patterns occur\.
+ The training file should be shuffled\. In other words, the time series should occur in a random order in the file\.
+ If you use the categorical feature \(`"cat"`\), all time series must have this feature\. It's required that you provide the range of `"cat"` \(see [DeepAR Hyperparameters](deepar_hyperparameters.md)\), and all values in such range must be present in the training data\.

If you specify optional test channel data, the DeepAR algorithm evaluates the trained model with different accuracy metrics\. The algorithm calculates the root mean square error \(RMSE\) over the test data as follows:

![\[Squrt(1/nT(Sum[i,t](y-hat(i,t)-y(i,t))^2))\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/deepar-1.png)

where *y**i*,*t* is the true value of time series *i* at time *t* and *ŷ**i*,*t* is the mean prediction\. The sum is over all *n* time series in the test set and over the last Τ time points for each time series, where Τ corresponds to the forecast horizon\. You specify the length of the forecast horizon by setting the `prediction_length` hyperparameter \(see [DeepAR Hyperparameters](deepar_hyperparameters.md)\)\.

In addition, the accuracy of the forecast distribution is evaluated using weighted quantile loss\. For a quantile in the range \[0, 1\], the weighted quantile loss is defined as follows:

![\[quantile loss\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/deepar-2.png)

Here, *q**i*,*t*\(τ\) is the τ\-quantile of the distribution that the model predicts\. Set the *test\_quantiles* hyperparameter to specify which quantiles for which the algorithm calculates quantile loss\. For information, see [DeepAR Hyperparameters](deepar_hyperparameters.md)\. 

If you have a set of time series, a simple way to prepare train and test datasets is as follows:
+ Use the full dataset in the test channel\.
+ In the train channel, remove the last `prediction_length` points from each time series\.

This ensures that the model does not see the removed points during training, and then those points are used for calculating the accuracy of the model\.

For inference, DeepAR accepts JSON format with an `"instances"` field which includes one or more time series in JSON Lines format, and a name of `"configuration"`, which includes parameters for generating the forecast\. For details, see [DeepAR Request and Response Formats](deepar-in-formats.md)\.

## DeepAR Instance Recommendations<a name="deepar-instances"></a>

You can train DeepAR on both GPU and CPU instances, in both single and multi\-machine settings\. We recommend starting with a single CPU instance \(for example, c4\.xlarge or c4\.2xlarge\), and switching to GPU instances and multiple machines only when necessary\. Using GPUs and multiple machines improves performance only when the model has more than 100 cells in each hidden layer and/or a mini\-batch size greater than 1000\.

For information on the mathematics behind DeepAR, see [DeepAR: Probabilistic Forecasting with Autoregressive Recurrent Networks](https://arxiv.org/abs/1704.04110)\. 

## DeepAR Common Questions<a name="deepar-faq"></a>

**Q: Can the model handle unobserved, missing values, or nan values?**

No, unobserved, missing values, and nan values are not currently supported\. 

**Q: Do all time series require the same length or the same starting point?**

No, the time series can have arbitrary starting points and arbitrary length\. \(Note, however, that time series shorter than `prediction_length` are ignored during training\)\.

**Q: Is there a one\-to\-one relation between the training set and the test set?**

No, time series in the training set are used to train the model\. After that, the trained model can be used to generate forecasts for the future of the time series used in the training set, or for other time series that were not previously included\.

**Q: Do I need to train one model per time series?**

We recommend that more than one time series be used when training a model\. 

**Q: Can I pass time\-dependent features or scalar features?**

Currently, only signel categorical features are supported\. In particular we do not support time\-dependent co\-variates\. However, time\-dependent features such as day of month are generated internally\. Because of this, it is important to set the `start` field to the right value rather than simply a dummy date\. 

**Q: How do I use the categorical feature?**

The categorical feature `cat` can be used to encode a grouping\. If the time series belong to `N` different groups, you can encode each such group by a number \(`0 to N - 1`\)\. The model can then use the categorical feature to generate better forecasts\. To use this feature, the parameter `cardinality` has to be set to the number of groups \(e\.g\. `N`\) and the `embedding_dimension` parameter also has to be set\. If any of these two hyperparameters are not set, then the `cat` field will be ignored. The embedding dimension is typically smaller than `cardinality`, for instance `log(N)`\. It is important to remember that, in the training set, all categories from `0 to N - 1` must be present in the training data or an otherwise an exception will be thrown\. This is occurs because during inference, we can only forecast for categories which we have previously seen in training\.

**Q: Can I pass multiple files?**

Yes\. The training folder and the test folder can each contain multiple files\. The file names can be arbitrary, but the file ending should be `.json`, `.gz.json` or `.parquet`\. For example: `s3://mybucket/myfolder/train/data-1.parquet` or `s3://mybucket/myfolder/train/data-2.parquet`\. 

**Q: Can I use this to train on a single time series?**

Models need sufficient data in order to learn typical behavior\. A single or small number of time series are typically not sufficient for training the neural network \(unless the time series are very long\)\. While a DeepAR model trained on a single time series will usually still generate sensible forecasts, standard forecasting methods such as ARIMA or ETS may be more accurate and stable\. Where the DeepAR approach starts to outperform the standard methods is when your dataset contains hundreds of time series and thus can be significantly more accurate with more data\. 

**Q: Do I need to split data into train/test set for evaluation?**

It is not necessary, but can be useful\. The time series in the `train` channel is used for training the model\. The time series in the `test` channel are used for evaluation after the model is trained\. For the evaluation, the last `prediction_length` points of each time series in the test set are withheld and a prediction is generated\. The forecast is then compared with the actual last `prediction_length` points\. Starting from a dataset of time series, the simplest train / test split can be created by using the entire dataset in the test channel \(in other words, all\-time series of full length\)\. For the train channel you can then remove the last `prediction_length` points from each time series so that the model does not see these points during training\. You can create more complex evaluations by repeating time series multiple times in the test set, but cutting them at different end points, resulting in accuracy metrics that are averaged over multiple forecasts from different time points\. 

**Q: Can the forecast horizon be changed later?**

No\. The forecast horizon \(`prediction_length`\) is fixed when a model is trained and it cannot be changed later\. 

**Q: Can the time series in the dataset have different frequencies?**

No, all time series in the dataset have to have the same frequency \(for example, hourly\)\.

**Q: Do I have to split my individual time series?**

No\. You should not split individual time series into pieces\. Each time series should be provided as a whole unit in the dataset \(see training format\)\. It is also important to make sure the start point is accurate for each time series\.

**Q: What is `context_length` and how should I set it?**

The `context_length` corresponds to the number of data points the algorithm gets to see before making a prediction\. Typical values are of the same order of magnitude as the forecast length\. Note that the algorithm also uses a set of so\-called "lags" that take into account observations that are farther back in time\. For instance, with a daily time series where you want to predict for one week, you might set the `context_length` to 14 days\. The lags are then automatically set depending on the frequency you set\. For daily data, in addition to current data, they will take into account observations 1 month previously as well as 1 year previously\. As a result, for the prediction, the algorithm will read in the last 14 days, 14 days one month ago, and 14 days one year ago\. Because of this, it is important to provide the entire time series when training and when doing inference\.

**Topics**
+ [Input/Output Interface](#deepar-inputoutput)
+ [DeepAR Instance Recommendations](#deepar-instances)
+ [DeepAR Common Questions](#deepar-faq)
+ [DeepAR Hyperparameters](deepar_hyperparameters.md)
+ [DeepAR Request and Response Formats](deepar-in-formats.md)
