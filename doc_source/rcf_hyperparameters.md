# RCF Hyperparameters<a name="rcf_hyperparameters"></a>

In the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request, you specify the training algorithm\. You can also specify algorithm\-specific hyperparameters as string\-to\-string maps\. The following table lists the hyperparameters for the Amazon SageMaker RCF algorithm\. For more information, including recommendations on how to choose hyperparameters, see [How RCF Works](rcf_how-it-works.md)\.


| Parameter Name | Description | 
| --- | --- | 
| feature\_dim |  The number of features in the data set\. \(If you use the [Random Cut Forest](https://sagemaker.readthedocs.io/en/stable/algorithms/randomcutforest.html) estimator, this value is calculated for you and need not be specified\.\) **Required** Valid values: Positive integer \(min: 1, max: 10000\)  | 
| eval\_metrics |  A list of metrics used to score a labeled test data set\. The following metrics can be selected for output: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/rcf_hyperparameters.html) **Optional** Valid values: a list with possible values taken from `accuracy` or `precision_recall_fscore`\.  Default value: Both `accuracy`, `precision_recall_fscore` are calculated\.  | 
| num\_samples\_per\_tree |  Number of random samples given to each tree from the training data set\. **Optional** Valid values: Positive integer \(min: 1, max: 2048\) Default value: 256  | 
| num\_trees |  Number of trees in the forest\. **Optional** Valid values: Positive integer \(min: 50, max: 1000\) Default value: 100  | 