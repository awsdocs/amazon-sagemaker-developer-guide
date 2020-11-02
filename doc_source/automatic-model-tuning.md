# Perform Automatic Model Tuning<a name="automatic-model-tuning"></a>

Amazon SageMaker automatic model tuning, also known as hyperparameter tuning, finds the best version of a model by running many training jobs on your dataset using the algorithm and ranges of hyperparameters that you specify\. It then chooses the hyperparameter values that result in a model that performs the best, as measured by a metric that you choose\.

For example, suppose that you want to solve a *[binary classification](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#binary-classification-model)* problem on a marketing dataset\. Your goal is to maximize the *[area under the curve \(auc\)](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#AUC)* metric of the algorithm by training an [XGBoost Algorithm](xgboost.md) model\. You don't know which values of the `eta`, `alpha`, `min_child_weight`, and `max_depth` hyperparameters to use to train the best model\. To find the best values for these hyperparameters, you can specify ranges of values that SageMaker hyperparameter tuning searches to find the combination of values that results in the training job that performs the best as measured by the objective metric that you chose\. Hyperparameter tuning launches training jobs that use hyperparameter values in the ranges that you specified, and returns the training job with highest auc\.

You can use SageMaker automatic model tuning with built\-in algorithms, custom algorithms, and SageMaker pre\-built containers for machine learning frameworks\.

Before you start using hyperparameter tuning, you should have a well\-defined machine learning problem, including the following:
+ A dataset
+ An understanding of the type of algorithm you need to train
+ A clear understanding of how you measure success

You should also prepare your dataset and algorithm so that they work in SageMaker and successfully run a training job at least once\. For information about setting up and running a training job, see [Get Started with Amazon SageMaker](gs.md)\.

**Topics**
+ [How Hyperparameter Tuning Works](automatic-model-tuning-how-it-works.md)
+ [Define Metrics](automatic-model-tuning-define-metrics.md)
+ [Define Hyperparameter Ranges](automatic-model-tuning-define-ranges.md)
+ [Tune Multiple Algorithms to Find the Best Model](multiple-algorithm-hpo.md)
+ [Example: Hyperparameter Tuning Job](automatic-model-tuning-ex.md)
+ [Stop Training Jobs Early](automatic-model-tuning-early-stopping.md)
+ [Run a Warm Start Hyperparameter Tuning Job](automatic-model-tuning-warm-start.md)
+ [Automatic Model Tuning Resource Limits](#automatic-model-tuning-limits)
+ [Best Practices for Hyperparameter Tuning](automatic-model-tuning-considerations.md)

## Automatic Model Tuning Resource Limits<a name="automatic-model-tuning-limits"></a>

SageMaker sets default limits for the following resources:
+ Number of concurrent hyperparameter tuning jobs \- 100
+ Number of hyperparameters that can be searched \- 20
**Note**  
Every possible value in a categorical hyperparameter counts against this limit\.
+ Number of metrics defined per hyperparameter tuning job \- 20
+ Number of concurrent training jobs per hyperparameter tuning job \- 10
+ Number of training jobs per hyperparameter tuning job \- 500
+ Maximum run time for a hyperparameter tuning job \- 30 days

 When you plan hyperparameter tuning jobs, you also have to take the limits on training resources into account\. For information about the default resource limits for SageMaker training jobs, see [SageMaker Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_sagemaker)\. Every concurrent training instance that all of your hyperparameter tuning jobs run on count against the total number of training instances allowed\. For example, suppose you run 10 concurrent hyperparameter tuning jobs\. Each of those hyperparameter tuning jobs runs 100 total training jobs, and runs 20 concurrent training jobs\. Each of those training jobs runs on one **ml\.m4\.xlarge** instance\. The following limits apply: 
+ Number of concurrent hyperparameter tuning jobs \- You don't need to increase the limit, because 10 tuning jobs is below the limit of 100\.
+ Number of training jobs per hyperparameter tuning job \- You don't need to increase the limit, because 100 training jobs is below the limit of 500\.
+ Number of concurrent training jobs per hyperparameter tuning job \- You need to request a limit increase to 20, because the default limit is 10\.
+ SageMaker training **ml\.m4\.xlarge** instances \- You need to request limit increase to 200, because you have 10 hyperparameter tuning jobs, with each of them running 20 concurrent training jobs\. The default limit is 20 instances\.
+ SageMaker training total instance count \- You need to request a limit increase to 200, because you have 10 hyperparameter tuning jobs, with each of them running 20 concurrent training jobs\. The default limit is 20 instances\.

For information about requesting limit increases for AWS resources, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.