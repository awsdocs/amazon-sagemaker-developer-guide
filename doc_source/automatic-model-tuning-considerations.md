# Best Practices for Hyperparameter Tuning<a name="automatic-model-tuning-considerations"></a>

Hyperparameter optimization is not a fully\-automated process\. To improve optimization, use the following guidelines when you create hyperparameters\.

**Topics**
+ [Choosing the Number of Hyperparameters](#automatic-model-tuning-num-hyperparameters)
+ [Choosing Hyperparameter Ranges](#automatic-model-tuning-choosing-ranges)
+ [Using Logarithmic Scales for Hyperparameters](#automatic-model-tuning-log-scales)
+ [Choosing the Best Number of Concurrent Training Jobs](#automatic-model-tuning-parallelism)
+ [Running Training Jobs on Multiple Instances](#automatic-model-tuning-distributed-metrics)

## Choosing the Number of Hyperparameters<a name="automatic-model-tuning-num-hyperparameters"></a>

The difficulty of a hyperparameter tuning job depends primarily on the number of hyperparameters that Amazon SageMaker has to search\. Although you can simultaneously use up to 20 variables in a hyperparameter tuning job, limiting your search to a much smaller number is likely to give better results\.

## Choosing Hyperparameter Ranges<a name="automatic-model-tuning-choosing-ranges"></a>

The range of values for hyperparameters that you choose to search can significantly affect the success of hyperparameter optimization\. Although you might want to specify a very large range that covers every possible value for a hyperparameter, you will get better results by limiting your search to a small range of values\. If you get the best metric values within a part of a range, consider limiting the range to that part\.

## Using Logarithmic Scales for Hyperparameters<a name="automatic-model-tuning-log-scales"></a>

During hyperparameter tuning, SageMaker attempts to ﬁgure out if your hyperparameters are log\-scaled or linear\-scaled\. Initially, it assumes that hyperparameters are linear\-scaled\. If they should be log\-scaled, it might take some time for SageMaker to discover that\. If you know that a hyperparameter should be log\-scaled and can convert it yourself, doing so could improve hyperparameter optimization\.

## Choosing the Best Number of Concurrent Training Jobs<a name="automatic-model-tuning-parallelism"></a>

Running more hyperparameter tuning jobs concurrently gets more work done quickly, but a tuning job improves only through successive rounds of experiments\. Typically, running one training job at a time achieves the best results with the least amount of compute time\.

## Running Training Jobs on Multiple Instances<a name="automatic-model-tuning-distributed-metrics"></a>

When a training job runs on multiple instances, hyperparameter tuning uses the last\-reported objective metric value from all instances of that training job as the value of the objective metric for that training job\. Design distributed training jobs so that the objective metric reported is the one that you want\.