# Monitoring a Model in Production<a name="how-it-works-model-monitor"></a>

After you deploy a model into your production environment, use Amazon SageMaker model monitor to continuously monitor the quality of your machine learning models in real time\. Amazon SageMaker model monitor enables you to set up an automated alert triggering system when there are deviations in the model quality, such as data drift and anomalies\. Amazon CloudWatch Logs collects log files of monitoring the model status and notifies when the quality of your model hits certain thresholds that you preset\. CloudWatch stores the log files to an Amazon S3 bucket you specify\. Early and pro\-active detection of model deviations through AWS model monitor products enables you to take prompt actions to maintain and improve the quality of your deployed model\. 

For more information about SageMaker model monitoring products, see [Monitor models for data and model quality, bias, and explainability](model-monitor.md)\.

To start your machine learning journey with SageMaker, sign up for an AWS account at [ Set Up SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-set-up.html)\. 