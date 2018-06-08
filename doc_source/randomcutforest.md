# Random Cut Forest<a name="randomcutforest"></a>

Amazon SageMaker Random Cut Forest \(RCF\) is an unsupervised algorithm for detecting anomalous data points within a data set\. These are observations which diverge from otherwise well\-structured or patterned data\. Anomalies can manifest as unexpected spikes in time series data, breaks in periodicity, or unclassifiable data points\. They are easy to describe in that, when viewed in a plot, they are often easily distinguishable from the "regular" data\. Including these anomalies in a data set can drastically increase the complexity of a machine learning task since the "regular" data can often be described with a simple model\.

With each data point, RCF associates an anomaly score\. Low score values indicate that the data point is considered "normal\." High values indicate the presence of an anomaly in the data\. The definitions of "low" and "high" depend on the application but common practice suggests that scores beyond three standard deviations from the mean score are considered anomalous\.

While there are many applications of anomaly detection algorithms to one\-dimensional time series data such as traffic volume analysis or sound volume spike detection, RCF is designed to work with arbitrary\-dimensional input\. Amazon SageMaker RCF scales well with respect to number of features, data set size, and number of instances\.

## Input/Output Interface<a name="rcf-input_output"></a>

Amazon SageMaker Random Cut Forest supports the `train` and `test` data channels\. The optional test channel is used to compute accuracy, precision, recall, and F1\-score metrics on labeled data\. Train and test data content types can be either `application/x-recordio-protobuf` or `text/csv` formats\. For the test data, when using text/csv format, the content must be specified as text/csv;label\_size=1 where the first column of each row represents the anomaly label: "1" for an anomalous data point and "0" for a normal data point\. RCF can be trained in File or Pipe mode when using recordIO\-wrapped protobuf, but only in File mode for the `CSV` format\.

Also note that the train channel only supports `S3DataDistributionType=ShardedByS3Key` and the test channel only supports `S3DataDistributionType=FullyReplicated`\. The S3 distribution type can be specified using the Python SDK as follows:

```
  import sagemaker
    
  # specify Random Cut Forest training job information and hyperparameters
  rcf = sagemaker.estimator.Estimator(...)
    
  # explicitly specify "SharededByS3Key" distribution type
  train_data = sagemaker.s3_input(
       s3_data=s3_training_data_location,
       content_type='text/csv;label_size=0',
       distribution='ShardedByS3Key')
    
  # run the training job on input data stored in S3
  rcf.fit({'train': train_data})
```

See the [Amazon SageMaker Data Types documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_S3DataSource.html) for more information on customizing the S3 data source attributes\. Finally, in order to take advantage of multi\-instance training the training data must be partitioned into at least as many files as instances\.

For inference, RCF supports `application/x-recordio-protobuf`, `text/csv` and `application/json` input data content types\. See the [Algorithms Provided by Amazon SageMaker: Common Data Formats](sagemaker-algo-common-data-formats.md) documentation for more information\. RCF inference returns `appplication/x-recordio-protobuf` or `application/json` formatted output\. Each record in these output data contains the corresponding anomaly scores for each input data point\. See [Common Data Formats\-\-Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html) for more information\.

Please see the [example notebooks](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/random_cut_forest/random_cut_forest.ipynb) for demonstrations of training and inference with different content types\.

## Instance Recommendations<a name="rcf-instance-recommend"></a>

For training, we recommend the `ml.m4`, `ml.c4`, and `ml.c5` instance families\. For inference we recommend using a `ml.c5.xl` instance type in particular, for maximum performance as well as minimized cost per hour of usage\. Although the algorithm could technically run on GPU instance types it does not take advantage of GPU hardware\.

**Topics**
+ [Input/Output Interface](#rcf-input_output)
+ [Instance Recommendations](#rcf-instance-recommend)
+ [How RCF Works](rcf_how-it-works.md)
+ [RCF Hyperparameters](rcf_hyperparameters.md)
+ [Tuning a RCF Model](random-cut-forest-tuning.md)
+ [RCF Response Formats](rcf-in-formats.md)