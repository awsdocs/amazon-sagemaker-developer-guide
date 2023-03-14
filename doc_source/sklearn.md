# Use Scikit\-learn with Amazon SageMaker<a name="sklearn"></a>

You can use Amazon SageMaker to train and deploy a model using custom Scikit\-learn code\. The SageMaker Python SDK Scikit\-learn estimators and models and the SageMaker open\-source Scikit\-learn containers make writing a Scikit\-learn script and running it in SageMaker easier\.

**Requirements**

Scikit\-learn 1\.0 has the following dependencies\.


| Dependency | Minimum version | 
| --- | --- | 
| Python | 3\.7 | 
| NumPy | 1\.14\.6 | 
| SciPy | 1\.1\.0 | 
| joblib | 0\.11 | 
| threadpoolctl | 2\.0\.0 | 

The SageMaker Scikit\-learn container supports the following Scikit\-learn versions\.


| Supported Scikit\-learn version | Minimum Python version | 
| --- | --- | 
| 1\.0\-1 | 3\.7 | 
| 0\.23\-1 | 3\.6 | 
| 0\.20\.0 | 2\.7 or 3\.4 | 

For general information about writing Scikit\-learn training scripts and using Scikit\-learn estimators and models with SageMaker, see [Using Scikit\-learn with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_sklearn.html)\.

## What do you want to do?<a name="sklearn-intent"></a>

**Note**  
Matplotlib v2\.2\.3 or newer is required to run the SageMaker Scikit\-learn example notebooks\.

I want to use Scikit\-learn for data processing, feature engineering, or model evaluation in SageMaker\.  
For a sample Jupyter notebook, see [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/sagemaker\_processing/scikit\_learn\_data\_processing\_and\_model\_evaluation](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/scikit_learn_data_processing_and_model_evaluation)\.  
For documentation, see [ReadTheDocs](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_processing.html#data-pre-processing-and-model-evaluation-with-scikit-learn)\.

I want to train a custom Scikit\-learn model in SageMaker\.  
For a sample Jupyter notebook, see [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/sagemaker\-python\-sdk/scikit\_learn\_iris](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-python-sdk/scikit_learn_iris)\.  
For documentation, see [Train a Model with Scikit\-learn](https://sagemaker.readthedocs.io/en/stable/using_sklearn.html#train-a-model-with-sklearn)\.

I have a Scikit\-learn model that I trained in SageMaker, and I want to deploy it to a hosted endpoint\.  
For more information, see [Deploy Scikit\-learn models](https://sagemaker.readthedocs.io/en/stable/using_sklearn.html#deploy-sklearn-models)\.

I have a Scikit\-learn model that I trained outside of SageMaker, and I want to deploy it to a SageMaker endpoint  
For more information, see [Deploy Endpoints from Model Data](https://sagemaker.readthedocs.io/en/stable/using_sklearn.html#deploy-endpoints-from-model-data)\.

I want to see the API documentation for [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) Scikit\-learn classes\.  
For more information, see [Scikit\-learn Classes](https://sagemaker.readthedocs.io/en/stable/sagemaker.sklearn.html)\.

I want to see information about SageMaker Scikit\-learn containers\.  
For more information, see [SageMaker Scikit\-learn Container GitHub repository](https://github.com/aws/sagemaker-scikit-learn-container)\.