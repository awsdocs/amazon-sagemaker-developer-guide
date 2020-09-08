# Feature Processing with Spark ML and Scikit\-learn<a name="inference-pipeline-mleap-scikit-learn-containers"></a>

Before training a model with either Amazon SageMaker built\-in algorithms or custom algorithms, you can use Spark and scikit\-learn preprocessors to transform your data and engineer features\. 

## Feature Processing with Spark ML<a name="feature-processing-spark"></a>

You can run Spark ML jobs with [AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html), a serverless ETL \(extract, transform, load\) service, from your SageMaker notebook\. You can also connect to existing EMR clusters to run Spark ML jobs with [Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html)\. To do this, you need an AWS Identity and Access Management \(IAM\) role that grants permission for making calls from your SageMaker notebook to AWS Glue\. 

**Note**  
To see which Python and Spark versions AWS Glue supports, refer to [ AWS Glue Release Notes](/glue/latest/dg/release-notes.html)\.

After engineering features, you package and serialize Spark ML jobs with MLeap into MLeap containers that you can add to an inference pipeline\. You don't need to use externally managed Spark clusters\. With this approach, you can seamlessly scale from a sample of rows to terabytes of data\. The same transformers work for both training and inference, so you don't need to duplicate preprocessing and feature engineering logic or develop a one\-time solution to make the models persist\. With inference pipelines, you don't need to maintain outside infrastructure, and you can make predictions directly from data inputs\.

When you run a Spark ML job on AWS Glue, a Spark ML pipeline is serialized into [MLeap](http://mleap-docs.combust.ml/) format\. Then, you can use the job with the [SparkML Model Serving Container](https://github.com/aws/sagemaker-sparkml-serving-container) in a SageMaker Inference Pipeline\.*MLeap* is a serialization format and execution engine for machine learning pipelines\. It supports Spark, Scikit\-learn, and TensorFlow for training pipelines and exporting them to a serialized pipeline called an MLeap Bundle\. You can deserialize Bundles back into Spark for batch\-mode scoring or into the MLeap runtime to power real\-time API services\.

## Feature Processing with Sci\-kit Learn<a name="feature-processing-with-scikit"></a>

You can run and package scikit\-learn jobs into containers directly in Amazon SageMaker\.  For an example of Python code for building a scikit\-learn featurizer model that trains on [Fisher's Iris flower data set](http://archive.ics.uci.edu/ml/datasets/Iris) and predicts the species of Iris based on morphological measurements, see [IRIS Training and Prediction with Sagemaker Scikit\-learn](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-python-sdk/scikit_learn_iris)\. 