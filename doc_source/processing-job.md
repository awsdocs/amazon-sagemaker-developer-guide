# Process Data and Evaluate Models<a name="processing-job"></a>

Use Amazon SageMaker Processing to analyze data and evaluate models on the Amazon SageMaker machine learning platform\. Amazon SageMaker is a fully managed service that covers the entire machine learning workflow, from preparing your data, to training and deploying the model to make predictions, and monitoring model performance when in production\. Data processing tasks such as feature engineering, data validation, model evaluation, and model interpretation are essential steps performed by engineers and data scientists in this machine learning workflow\. With Processing you can leverage a simplified, managed experience to run your data processing workloads on the Amazon SageMaker platform or by using the Amazon SageMaker APIs, in the experimentation phase and after code is deployed in production\. 

You can run an Amazon SageMaker Processing Job to process data from Amazon Simple Storage Service \(Amazon S3\), and save the processed data back to Amazon S3\.

![\[Running a processing job.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Processing-1.png)

**Topics**
+ [Amazon SageMaker Processing Sample Notebooks](#processing-job-sample-notebooks)
+ [Monitor Amazon SageMaker Processing with CloudWatch Logs and Metrics](#processing-job-cloudwatch)
+ [Data Processing and Model Evaluation with Scikit\-Learn](use-scikit-learn-processing-container.md)
+ [Use Your Own Processing Code](use-your-own-processing-code.md)

## Amazon SageMaker Processing Sample Notebooks<a name="processing-job-sample-notebooks"></a>

For a sample notebook that shows how to run scikit\-learn scripts to do data preprocessing and model evaluation with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) for Processing, see [scikit\-learn Processing](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/scikit_learn_data_processing_and_model_evaluation)\. This notebook also shows how to bring your own container to run processing workloads with your own dependencies\.

For a sample notebook that shows how to use Processing to do distributed data processing with Spark, see [Distributed Processing \(Spark\)](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/feature_transformation_with_sagemaker_processing)\.

For instructions on how to create and access Jupyter notebook instances that you can use to run the samples in Amazon SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all the Amazon SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.

## Monitor Amazon SageMaker Processing with CloudWatch Logs and Metrics<a name="processing-job-cloudwatch"></a>

Amazon SageMaker Processing provides Amazon CloudWatch logs and metrics to monitor processing jobs\. CloudWatch provides CPU, GPU, memory, GPU memory, and disk metrics and event logging\. For more information, see [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md) and [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\.