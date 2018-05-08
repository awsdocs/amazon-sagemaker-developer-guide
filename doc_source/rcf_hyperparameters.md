# RCF Hyperparameters<a name="rcf_hyperparameters"></a>

In the [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html) request, you specify the training algorithm\. You can also specify algorithm\-specific hyperparameters as string\-to\-string maps\. The following table lists the hyperparameters for the Amazon SageMaker RCF algorithm\. For more information, including recommendations on how to choose hyperparameters, see [How RCF Works](rcf_how-it-works.md)\.


| Parameter Name | Description | 
| --- | --- | 
| num\_samples\_per\_tree |  Number of random samples given to each tree from the training data set\. Valid values: Positive integer \(min: 1, max: 2048\) Default value: 256  | 
| num\_trees |  Number of trees in the forest\. Valid values: Positive integer \(min: 50, max: 1000\) Default value: 100  | 
| feature\_dim |  Number of features in the data set\. Valid values: Positive integer \(min: 1, max: 10000\) Default value: \-  | 
| eval\_metics |  List of metrics used to score a labeled test data set\. The following metrics can be selected for output: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/rcf_hyperparameters.html) Valid values: a list with possible values taken from `accuracy`, `precision_recall_fscore`\.  Default value: `accuracy`, `precision_recall_fscore`  | 