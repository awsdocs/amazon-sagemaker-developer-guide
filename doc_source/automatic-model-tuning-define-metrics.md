# Defining Objective Metrics<a name="automatic-model-tuning-define-metrics"></a>

**Note**  
When you use one of the Amazon SageMaker built\-in algorithms, you don't need to define metrics\. Built\-in algorithms automatically send metrics to hyperparameter tuning\. You do need to choose one of the metrics that the built\-in algorithm emits as the objective metric for the tuning job\. For a list of metrics that a built\-in algorithm emits, see the *Metrics* table for the algorithm in [Using Built\-in Algorithms with Amazon SageMaker](algos.md)\.

To optimize hyperparameters for a machine learning model, a tuning job evaluates the training jobs it launches by using a metric that the training algorithm writes to logs\. Amazon SageMaker hyperparameter tuning parses your algorithm’s `stdout` and `stderr` streams to find algorithm metrics, such as loss or validation\-accuracy, that show how well the model is performing on the dataset 

**Note**  
These are the same metrics that Amazon SageMaker sends to CloudWatch Logs\. For more information, see [Logging Amazon SageMaker with Amazon CloudWatch](logging-cloudwatch.md)\.

If you use your own algorithm for hyperparameter tuning, make sure that your algorithm emits at least one metric by writing evaluation data to `stderr` or `stdout`\.

You can define up to 20 metrics for your tuning job to monitor\. You choose one of those metrics to be the objective metric, which hyperparameter tuning uses to evaluate the training jobs\. The hyperparameter tuning job returns the training job that returned the best value for the objective metric as the best training job\.

You define metrics for a tuning job by specifying a name and a regular expression for each metric that your tuning job monitors\. Design the regular expressions to match metrics that your algorithm emits\. You pass these metrics to the [CreateHyperParameterTuningJob](API_CreateHyperParameterTuningJob.md) operation in the `TrainingJobDefinition` parameter as the `MetricDefinitions` field of the `AlgorithmSpecification` field\.

Metric definitions have the following structure:

```
[
  {
   "Name": "validation:rmse",
   "Regex": ".*\\[[0-9]+\\].*#011validation-rmse:(\\S+)"
  },
  {
   "Name": "validation:auc",
   "Regex": ".*\\[[0-9]+\\].*#011validation-auc:(\\S+)"
  },
  {
   "Name": "train:auc",
   "Regex": ".*\\[[0-9]+\\]#011train-auc:(\\S+).*"
  }
 ]
```

A regular expression \(regex\) matches what is in the log, like a search function\. Special characters in the regex affect the search\. For example, in the regex for the `valid-acc` metric defined above, the parentheses tell the regex to capture what's inside them\. We want it to capture the number, which is the metric\. The expression `[0-9\\.]+` inside the parentheses tells the regex to look for one or more occurrences of any digit or period \. The double backslash is an escape character that means "look for a literal period\." \(Normally, a regex interprets a period to mean match any single character\.\)

Choose one of the metrics that you define as the objective metric for the tuning job\. If you are using the Specify the value of the `name` key in the `HyperParameterTuningJobObjective` field of the `HyperParameterTuningJobConfig` parameter that you send to the [CreateHyperParameterTuningJob](API_CreateHyperParameterTuningJob.md)operation\.