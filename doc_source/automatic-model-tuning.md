# Perform Automatic Model Tuning with SageMaker<a name="automatic-model-tuning"></a>

Amazon SageMaker automatic model tuning, also known as hyperparameter tuning, finds the best version of a model by running many training jobs on your dataset using the algorithm and ranges of hyperparameters that you specify\. It then chooses the hyperparameter values that result in a model that performs the best, as measured by a metric that you choose\.

For example, suppose that you want to solve a *[binary classification](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#binary-classification-model)* problem on a marketing dataset\. Your goal is to maximize the *[area under the curve \(auc\)](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#AUC)* metric of the algorithm by training an [XGBoost Algorithm](xgboost.md) model\. You don't know which values of the `eta`, `alpha`, `min_child_weight`, and `max_depth` hyperparameters to use to train the best model\. To find the best values for these hyperparameters, you can specify ranges of values that SageMaker hyperparameter tuning searches to find the combination of values that results in the training job that performs the best as measured by the objective metric that you chose\. Hyperparameter tuning launches training jobs that use hyperparameter values in the ranges that you specified, and returns the training job with highest auc\.

You can use SageMaker automatic model tuning with built\-in algorithms, custom algorithms, and SageMaker pre\-built containers for machine learning frameworks\.

Amazon SageMaker automatic model tuning can use Amazon EC2 Spot instance to optimize costs when running training jobs\. For more information on managed spot training, see [Managed Spot Training in Amazon SageMaker](model-managed-spot-training.md)\.

Before you start using hyperparameter tuning, you should have a well\-defined machine learning problem, including the following:
+ A dataset
+ An understanding of the type of algorithm you need to train
+ A clear understanding of how you measure success

You should also prepare your dataset and algorithm so that they work in SageMaker and successfully run a training job at least once\. For information about setting up and running a training job, see [Get Started with Amazon SageMaker](gs.md)\.

**Topics**
+ [How Hyperparameter Tuning Works](automatic-model-tuning-how-it-works.md)
+ [Define Metrics](automatic-model-tuning-define-metrics.md)
+ [Define Hyperparameter Ranges](automatic-model-tuning-define-ranges.md)
+ [Tune Multiple Algorithms with Hyperparameter Optimization to Find the Best Model](multiple-algorithm-hpo.md)
+ [Example: Hyperparameter Tuning Job](automatic-model-tuning-ex.md)
+ [Stop Training Jobs Early](automatic-model-tuning-early-stopping.md)
+ [Run a Warm Start Hyperparameter Tuning Job](automatic-model-tuning-warm-start.md)
+ [Resource Limits for Automatic Model Tuning](automatic-model-tuning-limits.md)
+ [Best Practices for Hyperparameter Tuning](automatic-model-tuning-considerations.md)