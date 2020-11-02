# Example: Hyperparameter Tuning Job<a name="automatic-model-tuning-ex"></a>

This example shows how to create a new notebook for configuring and launching a hyperparameter tuning job\. The tuning job uses the [XGBoost Algorithm](xgboost.md) to train a model to predict whether a customer will enroll for a term deposit at a bank after being contacted by phone\.

You use the low\-level AWS SDK for Python \(Boto\) to configure and launch the hyperparameter tuning job, and the AWS Management Console to monitor the status of hyperparameter tuning jobs\. You can also use the Amazon SageMaker high\-level [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to configure, run, monitor, and analyze hyperparameter tuning jobs\. For more information, see [https://github\.com/aws/sagemaker\-python\-sdk](https://github.com/aws/sagemaker-python-sdk)\.

## Prerequisites<a name="automatic-model-tuning-ex-prereq"></a>

To run the code in this example, you need
+ [An AWS account and an administrator user](gs-set-up.md#gs-account-user)
+ [An Amazon S3 bucket for storing your training dataset and the model artifacts created during training](gs-config-permissions.md)
+ [A running SageMaker notebook instance](gs-setup-working-env.md)

**Topics**
+ [Prerequisites](#automatic-model-tuning-ex-prereq)
+ [Create a Notebook](automatic-model-tuning-ex-notebook.md)
+ [Get the Amazon SageMaker Boto 3 Client](automatic-model-tuning-ex-client.md)
+ [Get the SageMaker Execution Role](automatic-model-tuning-ex-role.md)
+ [Specify a Bucket and Data Output Location](automatic-model-tuning-ex-bucket.md)
+ [Download, Prepare, and Upload Training Data](automatic-model-tuning-ex-data.md)
+ [Configure and Launch a Hyperparameter Tuning Job](automatic-model-tuning-ex-tuning-job.md)
+ [Monitor the Progress of a Hyperparameter Tuning Job](automatic-model-tuning-monitor.md)
+ [Clean up](automatic-model-tuning-ex-cleanup.md)