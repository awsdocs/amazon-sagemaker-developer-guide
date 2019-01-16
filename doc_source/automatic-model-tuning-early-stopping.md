# Stop Training Jobs Early<a name="automatic-model-tuning-early-stopping"></a>

Stop the training jobs that a hyperparameter tuning job launches early when they are not improving significantly as measured by the objective metric\. Stopping training jobs early can help reduce compute time and helps you avoid overfitting your model\. To configure a hyperparameter tuning job to stop training jobs early, set the `TrainingJobEarlyStoppingType` field of the [HyperParameterTuningJobConfig](API_HyperParameterTuningJobConfig.md) object that you use to configure the tuning job to `AUTO`\. For information about configuring and launching a hyperparameter tuning job, see [Example: Hyperparameter Tuning Job](automatic-model-tuning-ex.md)\.

## How Early Stopping Works<a name="automatic-tuning-early-stop-how"></a>

When you enable early stopping for a hyperparameter tuning job, Amazon SageMaker evaluates each training job the hyperparameter tuning job launches as follows:
+ After each epoch of training, get the value of the objective metric\.
+ Compute the running average of the objective metric for all previous training jobs up to the same epoch, and then compute the median of all of the running averages\.
+ If the value of the objective metric for the current training job is worse \(higher when minimizing or lower when maximizing the objective metric\) than the median value of running averages of the objective metric for previous training jobs up to the same epoch, Amazon SageMaker stops the current training job\.

## Algorithms that Support Early Stopping<a name="automatic-tuning-early-stopping-algos"></a>

To support early stopping, an algorithm must emit objective metrics for each epoch\. The following built\-in Amazon SageMaker algorithms support early stopping:
+ [Linear Learner](linear-learner.md)—Supported only if you use `objective_loss` as the objective metric\.
+ [XGBoost Algorithm](xgboost.md)
+ [Image Classification Algorithm](image-classification.md)
+ [Object Detection Algorithm](object-detection.md)
+ [Sequence to Sequence ](seq-2-seq.md)
+ [IP Insights Algorithm](ip-insights.md)

**Note**  
This list of built\-in algorithms that support early stopping is current as of December 13, 2018\. Other built\-in algorithms might support early stopping in the future\. If an algorithm emits a metric that can be used as an objective metric for a hyperparameter tuning job \(preferably a validation metric\), then it supports early stopping\.

To use early stopping with your own algorithm, you must write your algorithms such that it emits the value of the objective metric after each epoch\. The following list shows how you can do that in different frameworks:

TensorFlow  
Use the t`f.contrib.learn.monitors.ValidationMonitor` class\. For information, see [https://www\.tensorflow\.org/api\_docs/python/tf/contrib/learn/monitors](https://www.tensorflow.org/api_docs/python/tf/contrib/learn/monitors)\.

MXNet  
Use the `mxnet.callback.LogValidationMetricsCallback`\. For information, see [https://mxnet\.apache\.org/api/python/callback/callback\.html](https://mxnet.apache.org/api/python/callback/callback.html)\.

Chainer  
Extend chainer by using the `extensions.Evaluator` class\. For information, see [https://docs\.chainer\.org/en/v1\.24\.0/reference/extensions\.html\#evaluator](https://docs.chainer.org/en/v1.24.0/reference/extensions.html#evaluator)\.

PyTorch and Spark  
There is no high\-level support\. You must explicitly write your training code so that it computes objective metrics and writes them to logs after each epoch\.