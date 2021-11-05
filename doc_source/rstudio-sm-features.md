# Access Amazon SageMaker features with RStudio on Amazon SageMaker<a name="rstudio-sm-features"></a>

 One of the benefits of using RStudio on Amazon SageMaker is the integration of Amazon SageMaker features\. This includes integration with Amazon SageMaker Studio and Reticulate\. 

 **Use Amazon SageMaker Studio JupyterLab and RStudio on Amazon SageMaker** 

 Your Amazon SageMaker Studio JupyterLab and RStudio instances share the same Amazon EFS file system\. This means that files that you import and create using JupyterLab can be accessed using RStudio and vice versa\. This allows you to work on the same files using both JupyterLab and RStudio without having to move your files between the two\. For more information on this workflow, see the [Announcing Fully Managed RStudio on Amazon SageMaker for Data Scientists](http://aws.amazon.com/blogs/aws/announcing-fully-managed-rstudio-on-amazon-sagemaker-for-data-scientists) blog\.

 **Use Amazon SageMaker SDK with reticulate** 

The [reticulate](https://rstudio.github.io/reticulate) package is used as an R interface to [Amazon SageMaker Python SDK](http://sagemaker.readthedocs.io/en/latest/index.html) to make API calls to Amazon SageMaker\. The reticulate package translates between R and Python objects, and Amazon SageMaker provides a serverless data science environment to train and deploy Machine Learning \(ML\) models at scale\. For general information about the reticulate package, see [ R Interface to Python](https://rstudio.github.io/reticulate/)\.

For a blog that outlines how to use the reticulate package with Amazon SageMaker, see [Using R with Amazon SageMaker](http://aws.amazon.com/blogs/machine-learning/using-r-with-amazon-sagemaker/)\.

The following examples show how to use reticulate for specific use cases\.
+ For a notebook that describes how to use reticulate to do batch transform to make predictions, see [Batch Transform Using R with Amazon SageMaker](https://sagemaker-examples.readthedocs.io/en/latest/r_examples/r_batch_transform/r_xgboost_batch_transform.html)\.
+ For a notebook that describes how to use reticulate to conduct hyperparameter tuning and generate predictions, see [Hyperparameter Optimization Using R with Amazon SageMaker](https://sagemaker-examples.readthedocs.io/en/latest/r_examples/r_xgboost_hpo_batch_transform/r_xgboost_hpo_batch_transform.html)\.