# Define Metrics<a name="automatic-model-tuning-define-metrics"></a>

**Note**  
When you use one of the Amazon SageMaker built\-in algorithms, you don't need to define metrics\. Built\-in algorithms automatically send metrics to hyperparameter tuning\. You do need to choose one of the metrics that the built\-in algorithm emits as the objective metric for the tuning job\. For a list of metrics that a built\-in algorithm emits, see the *Metrics* table for the algorithm in [Use Amazon SageMaker built\-in algorithms](algos.md)\.

To optimize hyperparameters for a machine learning model, a tuning job evaluates the training jobs it launches by using a metric that the training algorithm writes to logs\. Amazon SageMaker hyperparameter tuning parses your algorithm’s `stdout` and `stderr` streams to find algorithm metrics, such as loss or validation\-accuracy, that show how well the model is performing on the dataset 

**Note**  
These are the same metrics that SageMaker sends to CloudWatch Logs\. For more information, see [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\.

If you use your own algorithm for hyperparameter tuning, make sure that your algorithm emits at least one metric by writing evaluation data to `stderr` or `stdout`\.

**Note**  
Hyperparameter tuning sends an additional hyperparameter, `_tuning_objective_metric` to the training algorithm\. This hyperparameter specifies the objective metric being used for the hyperparameter tuning job, so that your algorithm can use that information during training\.

You can define up to 20 metrics for your tuning job to monitor\. You choose one of those metrics to be the objective metric, which hyperparameter tuning uses to evaluate the training jobs\. The hyperparameter tuning job returns the training job that returned the best value for the objective metric as the best training job\.

You define metrics for a tuning job by specifying a name and a regular expression for each metric that your tuning job monitors\. Design the regular expressions to capture the values of metrics that your algorithm emits\. You pass these metrics to the [ `CreateHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html) operation in the `TrainingJobDefinition` parameter as the `MetricDefinitions` field of the `AlgorithmSpecification` field\.

The following example defines 4 metrics:

```
=[
    {
        "Name": "loss",
        "Regex": "Loss = (.*?);",
    },
    {
        "Name": "ganloss",
        "Regex": "GAN_loss=(.*?);",
    },
    {
        "Name": "discloss",
        "Regex": "disc_train_loss=(.*?);",
    },
    {
        "Name": "disc-combined",
        "Regex": "disc-combined=(.*?);",
    },
]
```

The following is an example of the log that the algorithm writes:

```
GAN_loss=0.138318;  Scaled_reg=2.654134; disc:[-0.017371,0.102429] real 93.3% gen 0.0% disc-combined=0.000000; disc_train_loss=1.374587;  Loss = 16.020744;  Iteration 0 took 0.704s;  Elapsed=0s
```

Use the regular expression \(regex\) to match the algorithm's log output and capture the numeric values of metrics\. For example, in the regex for the `loss` metric defined above, the first part of the regex finds the exact text "Loss = ", and the expression `(.*?);` captures zero or more of any character until the first semicolon character\. In this expression, the parenthesis tell the regex to capture what is inside them, `.` means any character, `*` means zero or more, and `?` means capture only until the first instance of the `;` character\.

Choose one of the metrics that you define as the objective metric for the tuning job\. If you are using the API, specify the value of the `name` key in the `HyperParameterTuningJobObjective` field of the `HyperParameterTuningJobConfig` parameter that you send to the [ `CreateHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html) operation\.