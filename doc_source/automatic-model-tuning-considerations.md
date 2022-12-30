# Best Practices for Hyperparameter Tuning<a name="automatic-model-tuning-considerations"></a>

Hyperparameter optimization \(HPO\) is not a fully\-automated process\. To improve optimization, follow these best practices for hyperparameter tuning\.

**Topics**
+ [Choosing a tuning strategy](#automatic-model-tuning-strategy)
+ [Choosing the number of hyperparameters](#automatic-model-tuning-num-hyperparameters)
+ [Choosing hyperparameter ranges](#automatic-model-tuning-choosing-ranges)
+ [Using the correct scales for hyperparameters](#automatic-model-tuning-log-scales)
+ [Choosing the best number of parallel training jobs](#automatic-model-tuning-parallelism)
+ [Running training jobs on multiple instances](#automatic-model-tuning-distributed-metrics)
+ [Using a random seed to reproduce hyperparameter configurations](#automatic-model-tuning-random-seed)

## Choosing a tuning strategy<a name="automatic-model-tuning-strategy"></a>

For large jobs, using the [Hyperband](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-how-it-works.html#automatic-tuning-hyperband) tuning strategy can reduce computation time\. Hyperband has an early stopping mechanism to stop under\-performing jobs\. Hyperband can also reallocate resources towards well\-utilized hyperparameter configurations and run parallel jobs\. For smaller training jobs using less runtime, use either [random search](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-how-it-works.html#automatic-tuning-random-search) or [Bayesian optimization](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-how-it-works.html#automatic-tuning-bayesian-optimization.title)\. 

Use Bayesian optimization to make increasingly informed decisions about improving hyperparameter configurations in the next run\. Bayesian optimization uses information gathered from prior runs to improve subsequent runs\. Because of its sequential nature, Bayesian optimization cannot massively scale\. 

Use random search to run a large number of parallel jobs\. In random search, subsequent jobs do not depend on the results from prior jobs and can be run independently\. Compared to other strategies, random search is able to run the largest number of parallel jobs\. 

Use [grid search](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-how-it-works.html#automatic-tuning-grid-search) to reproduce results of a tuning job, or if simplicity and transparency of the optimization algorithm are important\. You can also use grid search to explore the entire hyperparameter search space evenly\. Grid search methodically searches through every hyperparameter combination to find optimal hyperparameter values\. Unlike grid search, Bayesian optimization, random search and Hyperband all draw hyperparameters randomly from the search space\. Because grid search analyzes every combination of hyperparameters, optimal hyperparameter values will be identical between tuning jobs that use the same hyperparameters\. 

## Choosing the number of hyperparameters<a name="automatic-model-tuning-num-hyperparameters"></a>

During optimization, the computational complexity of a hyperparameter tuning job depends on the following:
+ The number of hyperparameters
+ The range of values that Amazon SageMaker has to search

Although you can simultaneously specify up to 30 hyperparameters, limiting your search to a smaller number can reduce computation time\. Reducing computation time allows SageMaker to converge more quickly to an optimal hyperparameter configuration\.

## Choosing hyperparameter ranges<a name="automatic-model-tuning-choosing-ranges"></a>

The range of values that you choose to search can adversely affect hyperparameter optimization\. For example, a range that covers every possible hyperparameter value can lead to large compute times and a model that doesn't generalize well to unseen data\. If you know that using a subset of the largest possible range is appropriate for your use case, consider limiting the range to that subset\.

## Using the correct scales for hyperparameters<a name="automatic-model-tuning-log-scales"></a>

During hyperparameter tuning, SageMaker attempts to infer if your hyperparameters are log\-scaled or linear\-scaled\. Initially, SageMaker assumes linear scaling for hyperparameters\. If hyperparameters are log\-scaled, choosing the correct scale will make your search more efficient\. You can also select `Auto` for `ScalingType` in the [CreateHyperParameterTuningJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html) API if you want SageMaker to detect the scale for you\.

## Choosing the best number of parallel training jobs<a name="automatic-model-tuning-parallelism"></a>

You can use the results of previous trials to improve the performance of subsequent trials\. Choose the largest number of parallel jobs that would provide a meaningful incremental result that is also within your region and account compute constraints\. Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResourceLimits.html#MaxParallelTrainingJobs](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResourceLimits.html#MaxParallelTrainingJobs) field to limit the number of training jobs that a hyperparameter tuning job can launch in parallel\. For more information, see [Running multiple HPO jobs in parallel on Amazon SageMaker](http://aws.amazon.com/blogs/machine-learning/running-multiple-hpo-jobs-in-parallel-on-amazon-sagemaker)\.

## Running training jobs on multiple instances<a name="automatic-model-tuning-distributed-metrics"></a>

When a training job runs on multiple machines in distributed mode, each machine emits an objective metric\. HPO can only use one of these emitted objective metrics to evaluate model performance, In distributed mode, HPO uses the objective metric that was reported by the last running job across all instances\. 

## Using a random seed to reproduce hyperparameter configurations<a name="automatic-model-tuning-random-seed"></a>

You can specify an integer as a random seed for hyperparameter tuning and use that seed during hyperparameter generation\. Later, you can use the same seed to reproduce hyperparameter configurations that are consistent with your previous results\. For random search and Hyperband strategies, using the same random seed can provide up to 100% reproducibility of the previous hyperparameter configuration for the same tuning job\. For Bayesian strategy, using the same random seed will improve reproducibility for the same tuning job\.