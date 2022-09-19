# Best Practices for Hyperparameter Tuning<a name="automatic-model-tuning-considerations"></a>

Hyperparameter optimization is not a fully\-automated process\. To improve optimization, use the following guidelines when you create hyperparameters\.

**Topics**
+ [Choosing a Strategy](#automatic-model-tuning-strategy)
+ [Choosing the Number of Hyperparameters](#automatic-model-tuning-num-hyperparameters)
+ [Choosing Hyperparameter Ranges](#automatic-model-tuning-choosing-ranges)
+ [Using Logarithmic Scales for Hyperparameters](#automatic-model-tuning-log-scales)
+ [Choosing the Best Number of Concurrent Training Jobs](#automatic-model-tuning-parallelism)
+ [Running Training Jobs on Multiple Instances](#automatic-model-tuning-distributed-metrics)

## Choosing a Strategy<a name="automatic-model-tuning-strategy"></a>

For large jobs, using Hyperband can reduce computation time by utilizing its internal early stopping mechanism, reallocation of resources and ability to run parallel jobs\. If runtime and resources are limited, use either random search or Bayesian optimization instead\. Bayesian optimization uses information gathered from prior runs to make increasingly informed decisions about improving hyperparameter configurations in the next run\. Because of its sequential nature, Bayesian optimization cannot massively scale\. Random search is able to run large numbers of parallel jobs\.

## Choosing the Number of Hyperparameters<a name="automatic-model-tuning-num-hyperparameters"></a>

The computational complexity of a hyperparameter tuning job depends primarily on the number of hyperparameters whose range of values Amazon SageMaker has to search through during optimization\. Although you can simultaneously specify up to 20 hyperparameters to optimize for a tuning job, limiting your search to a much smaller number is likely to give you better results\.

## Choosing Hyperparameter Ranges<a name="automatic-model-tuning-choosing-ranges"></a>

The range of values for hyperparameters that you choose to search can significantly affect the success of hyperparameter optimization\. Although you might want to specify a very large range that covers every possible value for a hyperparameter, you get better results by limiting your search to a small range of values\. If you know that you get the best metric values within a subset of the possible range, consider limiting the range to that subset\.

## Using Logarithmic Scales for Hyperparameters<a name="automatic-model-tuning-log-scales"></a>

During hyperparameter tuning, SageMaker attempts to ﬁgure out if your hyperparameters are log\-scaled or linear\-scaled\. Initially, it assumes that hyperparameters are linear\-scaled\. If they are in fact log\-scaled, it might take some time for SageMaker to discover that fact\. If you know that a hyperparameter is log\-scaled and can convert it yourself, doing so could improve hyperparameter optimization\.

## Choosing the Best Number of Concurrent Training Jobs<a name="automatic-model-tuning-parallelism"></a>

When setting the resource limit [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResourceLimits.html#MaxParallelTrainingJobs](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResourceLimits.html#MaxParallelTrainingJobs) for the maximum number of concurrent training jobs that a hyperparameter tuning job can launch, consider the following tradeoff\. Running more hyperparameter tuning jobs concurrently gets more work done quickly, but a tuning job improves only through successive rounds of experiments\. Typically, running one training job at a time achieves the best results with the least amount of compute time\.

## Running Training Jobs on Multiple Instances<a name="automatic-model-tuning-distributed-metrics"></a>

When a training job runs on multiple instances, hyperparameter tuning uses the last\-reported objective metric value from all instances of that training job as the value of the objective metric for that training job\. Design distributed training jobs so that the objective metric reported is the one that you want\.