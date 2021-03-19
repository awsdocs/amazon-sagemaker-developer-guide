# Get Started with Amazon SageMaker Notebook Instances<a name="gs-console"></a>

One of the best ways for machine learning \(ML\) practitioners to use Amazon SageMaker is to train and deploy ML models using SageMaker notebook instances\. The SageMaker notebook instances help create the environment by initiating Jupyter servers on Amazon Elastic Compute Cloud \(Amazon EC2\) and providing preconfigured kernels with the following packages: the Amazon SageMaker Python SDK, AWS SDK for Python \(Boto3\), AWS Command Line Interface \(AWS CLI\), Conda, Pandas, deep learning framework libraries, and other libraries for data science and machine learning\.

## Machine Learning with the SageMaker Python SDK<a name="gs-ml-with-sagemaker-pysdk"></a>

To train, validate, deploy, and evaluate an ML model in a SageMaker notebook instance, use the SageMaker Python SDK that abstracts AWS SDK for Python \(Boto3\) and SageMaker API operations\. It enables you to orchestrate the AWS infrastructures, such as Amazon Simple Storage Service \(Amazon S3\) for saving data and model artifacts, Amazon Elastic Container Registry \(ECR\) for importing and servicing the ML models, Amazon Elastic Compute Cloud \(Amazon EC2\) for training and inference\.

You can also take advantage of SageMaker features that help you deal with every stage of a complete ML cycle: data labeling, data preprocessing, model training, model deployment, evaluation on prediction performance, and monitoring the quality of model in production\.

If you're a first\-time SageMaker user, we recommend you to use the SageMaker Python SDK, following the end\-to\-end ML tutorial\. To find the open source documentation, see the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

## Tutorial Overview<a name="gs-tutorial-overview"></a>

This Get Started tutorial walks you through how to create a SageMaker notebook instance, open a Jupyter notebook with a preconfigured kernel with the Conda environment for machine learning, and start a SageMaker session to run an end\-to\-end ML cycle\. You'll learn how to save a dataset to a default Amazon S3 bucket automatically paired with the SageMaker session, submit a training job of an ML model to Amazon EC2, and deploy the trained model for prediction by hosting or batch inferencing through Amazon EC2\. 

This tutorial explicitly shows a complete ML flow of training the XGBoost model from the SageMaker built\-in model pool\. You use the [US Adult Census dataset](https://archive.ics.uci.edu/ml/datasets/adult), and you evaluate the performance of the trained SageMaker XGBoost model on predicting individuals' income\.
+ [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) – The [XGBoost](https://xgboost.readthedocs.io/en/latest/) model is adapted to the SageMaker environment and preconfigured as Docker containers\. SageMaker provides a suite of [built\-in algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html) that are prepared for using SageMaker features\. To learn more about what ML algorithms are adapted to SageMaker, see [Choose an Algorithm](https://docs.aws.amazon.com/sagemaker/latest/dg/algorithms-choose.html) and [Use Amazon SageMaker Built\-in Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\. For the SageMaker built\-in algorithm API operations, see [First\-Party Algorithms](https://sagemaker.readthedocs.io/en/stable/algorithms/index.html) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.
+ [Adult Census dataset](https://archive.ics.uci.edu/ml/datasets/adult) – The dataset from the [1994 Census bureau database](http://www.census.gov/en.html) by Ronny Kohavi and Barry Becker \(Data Mining and Visualization, Silicon Graphics\)\. The SageMaker XGBoost model is trained using this dataset to predict if an individual makes over $50,000 a year or less\.

**Topics**
+ [Machine Learning with the SageMaker Python SDK](#gs-ml-with-sagemaker-pysdk)
+ [Tutorial Overview](#gs-tutorial-overview)
+ [Step 1: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)
+ [Step 2: Create a Jupyter Notebook](ex1-prepare.md)
+ [Step 3: Download, Explore, and Transform a Dataset](ex1-preprocess-data.md)
+ [Step 4: Train a Model](ex1-train-model.md)
+ [Step 5: Deploy the Model to Amazon EC2](ex1-model-deployment.md)
+ [Step 6: Evaluate the Model](ex1-test-model.md)
+ [Step 7: Clean Up](ex1-cleanup.md)