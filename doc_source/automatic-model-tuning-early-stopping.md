# Stop Training Jobs Early<a name="automatic-model-tuning-early-stopping"></a>

Stop the training jobs that a hyperparameter tuning job launches early when they are not improving significantly as measured by the objective metric\. Stopping training jobs early can help reduce compute time and helps you avoid overfitting your model\. To configure a hyperparameter tuning job to stop training jobs early, do one of the following:
+ If you are using the AWS SDK for Python \(Boto 3\), set the `TrainingJobEarlyStoppingType` field of the [ `HyperParameterTuningJobConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HyperParameterTuningJobConfig.html) object that you use to configure the tuning job to `AUTO`\.
+ If you are using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), set the `early_stopping_type` parameter of the [HyperParameterTuner](https://sagemaker.readthedocs.io/en/stable/tuner.html) object to `Auto`\.
+ In the Amazon SageMaker console, in the **Create hyperparameter tuning job** workflow, under **Early stopping**, choose **Auto**\.

For a sample notebook that demonstrates how to use early stopping, see [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/hyperparameter\_tuning/image\_classification\_early\_stopping/hpo\_image\_classification\_early\_stopping\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/hyperparameter_tuning/image_classification_early_stopping/hpo_image_classification_early_stopping.ipynb) or open the `hpo_image_classification_early_stopping.ipynb` notebook in the **Hyperparameter Tuning** section of the **SageMaker Examples** in a notebook instance\. For information about using sample notebooks in a notebook instance, see [Example Notebooks](howitworks-nbexamples.md)\.

## How Early Stopping Works<a name="automatic-tuning-early-stop-how"></a>

When you enable early stopping for a hyperparameter tuning job, SageMaker evaluates each training job the hyperparameter tuning job launches as follows:
+ After each epoch of training, get the value of the objective metric\.
+ Compute the running average of the objective metric for all previous training jobs up to the same epoch, and then compute the median of all of the running averages\.
+ If the value of the objective metric for the current training job is worse \(higher when minimizing or lower when maximizing the objective metric\) than the median value of running averages of the objective metric for previous training jobs up to the same epoch, SageMaker stops the current training job\.

## Algorithms That Support Early Stopping<a name="automatic-tuning-early-stopping-algos"></a>

To support early stopping, an algorithm must emit objective metrics for each epoch\. The following built\-in SageMaker algorithms support early stopping:
+ [Linear learner algorithm](linear-learner.md)—Supported only if you use `objective_loss` as the objective metric\.
+ [XGBoost Algorithm](xgboost.md)
+ [Image Classification Algorithm](image-classification.md)
+ [Object Detection Algorithm](object-detection.md)
+ [Sequence\-to\-Sequence Algorithm](seq-2-seq.md)
+ [IP Insights Algorithm](ip-insights.md)

**Note**  
This list of built\-in algorithms that support early stopping is current as of December 13, 2018\. Other built\-in algorithms might support early stopping in the future\. If an algorithm emits a metric that can be used as an objective metric for a hyperparameter tuning job \(preferably a validation metric\), then it supports early stopping\.

To use early stopping with your own algorithm, you must write your algorithms such that it emits the value of the objective metric after each epoch\. The following list shows how you can do that in different frameworks:

TensorFlow  
Use the `tf.keras.callbacks.ProgbarLogger` class\. For information, see [https://www\.tensorflow\.org/api\_docs/python/tf/keras/callbacks/ProgbarLogger](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/ProgbarLogger)\.

MXNet  
Use the `mxnet.callback.LogValidationMetricsCallback`\. For information, see [https://mxnet\.apache\.org/api/python/callback/callback\.html](https://mxnet.apache.org/api/python/callback/callback.html)\.

Chainer  
Extend chainer by using the `extensions.Evaluator` class\. For information, see [https://docs\.chainer\.org/en/v1\.24\.0/reference/extensions\.html\#evaluator](https://docs.chainer.org/en/v1.24.0/reference/extensions.html#evaluator)\.

PyTorch and Spark  
There is no high\-level support\. You must explicitly write your training code so that it computes objective metrics and writes them to logs after each epoch\.