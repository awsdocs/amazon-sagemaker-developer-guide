# Process Data and Evaluate Models<a name="processing-job"></a>

To analyze data and evaluate machine learning models on Amazon SageMaker, use Amazon SageMaker Processing\. With Processing, you can use a simplified, managed experience on SageMaker to run your data processing workloads, such as feature engineering, data validation, model evaluation, and model interpretation\. You can also use the Amazon SageMaker Processing APIs during the experimentation phase and after the code is deployed in production to evaluate performance\. 

![\[Running a processing job.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Processing-1.png)

The preceding diagram shows how Amazon SageMaker spins up a Processing job\. Amazon SageMaker takes your script, copies your data from Amazon Simple Storage Service \(Amazon S3\), and then pulls a processing container\. The processing container image can either be an Amazon SageMaker built\-in image or a custom image that you provide\. The underlying infrastructure for a Processing job is fully managed by Amazon SageMaker\. Cluster resources are provisioned for the duration of your job, and cleaned up when a job completes\. The output of the Processing job is stored in the Amazon S3 bucket you specified\. 

**Note**  
Your data must be stored in an Amazon S3 bucket\.

**Topics**
+ [Use Amazon SageMaker Processing Sample Notebooks](#processing-job-sample-notebooks)
+ [Monitor Amazon SageMaker Processing Jobs with CloudWatch Logs and Metrics](#processing-job-cloudwatch)
+ [Process Data and Evaluate Models with scikit\-learn](use-scikit-learn-processing-container.md)
+ [Use Your Own Processing Code](use-your-own-processing-code.md)

## Use Amazon SageMaker Processing Sample Notebooks<a name="processing-job-sample-notebooks"></a>

We provide two sample Jupyter notebooks that show how to perform data preprocessing, model evaluation, or both\.

For a sample notebook that shows how to run scikit\-learn scripts to perform data preprocessing and model training and evaluation with the SageMaker Python SDK for Processing, see [scikit\-learn Processing](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/scikit_learn_data_processing_and_model_evaluation)\. This notebook also shows how to use your own custom container to run processing workloads with your Python libraries and other specific dependencies\.

For a sample notebook that shows how to use Amazon SageMaker Processing to perform distributed data preprocessing with Spark, see [Distributed Processing \(Spark\)](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/feature_transformation_with_sagemaker_processing)\. This notebook also shows how to train a regression model using XGBoost on the preprocessed dataset\.

For instructions on how to create and access Jupyter notebook instances that you can use to run these samples in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all the SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.

## Monitor Amazon SageMaker Processing Jobs with CloudWatch Logs and Metrics<a name="processing-job-cloudwatch"></a>

Amazon SageMaker Processing provides Amazon CloudWatch logs and metrics to monitor processing jobs\. CloudWatch provides CPU, GPU, memory, GPU memory, and disk metrics, and event logging\. For more information, see [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md) and [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\.