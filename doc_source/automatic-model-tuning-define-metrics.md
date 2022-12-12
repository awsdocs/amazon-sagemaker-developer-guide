# Define Metrics<a name="automatic-model-tuning-define-metrics"></a>

A tuning job optimizes hyperparameters for training jobs that it launches by using a metric to evaluate performance\. This guide shows how to define metrics so that you can use a custom algorithm for training, or use a built\-in algorithm from Amazon SageMaker\. 

Amazon SageMaker hyperparameter tuning parses your machine learning algorithm's `stdout` and `stderr` streams to find metrics, such as loss or validation\-accuracy\. The metrics show how well the model is performing on the dataset\. 

The following sections describe how to use two types of algorithms for training: built\-in and custom\.

## Use a built\-in algorithm for training<a name="automatic-model-tuning-define-metrics-builtin"></a>

If you use one of the [SageMaker built\-in algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html), metrics are already defined for you\. In addition, built\-in algorithms automatically send metrics to hyperparameter tuning for optimization\. These metrics are also written to Amazon CloudWatch logs\. For more information, see [Log Amazon SageMaker Events with Amazon CloudWatch](https://docs.aws.amazon.com/sagemaker/latest/dg/logging-cloudwatch.html)\. 

For the objective metric for the tuning job, choose one of the metrics that the built\-in algorithm emits\. For a list of available metrics, see the model tuning section for the appropriate algorithm in [Use Amazon SageMaker Built\-in Algorithms or Pre\-trained Models](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.

You can choose up to 40 metrics to monitor in your [tuning job](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HyperParameterAlgorithmSpecification.html)\. Select one of those metrics to be the objective metric\. The hyperparameter tuning job returns the [training job ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html#sagemaker-DescribeHyperParameterTuningJob-response-BestTrainingJob) that performed the best against the objective metric\.

**Note**  
Hyperparameter tuning automatically sends an additional hyperparameter `_tuning_objective_metric` to pass your objective metric to the tuning job for use during training\.

## Use a custom algorithm for training<a name="automatic-model-tuning-define-metrics-custom"></a>

This section shows how to define your own metrics to use your own custom algorithm for training\. When doing so, make sure that your algorithm writes at least one metric to `stderr` or `stdout`\. Hyperparameter tuning parses these streams to find algorithm metrics that show how well the model is performing on the dataset\.

You can define custom metrics by specifying a name and regular expression for each metric that your tuning job monitors\. Then, pass these metric definitions to the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html) API in the `TrainingJobDefinition` parameter in the `MetricDefinitions` field of `AlgorithmSpecification`\.

The following shows sample output from a log written to `stderr` or `stdout` by a training algorithm\.

```
GAN_loss=0.138318;  Scaled_reg=2.654134; disc:[-0.017371,0.102429] real 93.3% gen 0.0% disc-combined=0.000000; disc_train_loss=1.374587;  Loss = 16.020744;  Iteration 0 took 0.704s;  Elapsed=0s
```

The following code example shows how to use regular expressions in Python \(regex\)\. This is used to search the sample log output and capture the numeric values of four different metrics\.

```
[
    {
        "Name": "ganloss",
        "Regex": "GAN_loss=(.*?);",
    },
    {
        "Name": "disc-combined",
        "Regex": "disc-combined=(.*?);",
    },
    {
        "Name": "discloss",
        "Regex": "disc_train_loss=(.*?);",
    },
    {
        "Name": "loss",
        "Regex": "Loss = (.*?);",
    },
]
```

In regular expressions, parenthesis `()` are used to group parts of the regular expression together\.
+ For the `loss` metric that is defined in the code example, the expression `(.*?);` captures any character between the exact text `"Loss="` and the first semicolon \(`;`\) character\.
+ The character `.` instructs the regular expression to match any character\.
+  The character `*` means to match zero or more characters\. 
+ The character `?` means capture only until the first instance of the `;` character\. 

The loss metric defined in the code sample will capture `Loss = 16.020744` from the sample output\.

Choose one of the metrics that you define as the objective metric for the tuning job\. If you are using the SageMaker API, specify the value of the `name` key in the `HyperParameterTuningJobObjective` field of the `HyperParameterTuningJobConfig` parameter that you send to the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html) operation\.