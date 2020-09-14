# R User Guide to Amazon SageMaker<a name="r-guide"></a>

 This document will walk you through ways of leveraging Amazon SageMaker features using R\. This guide introduces SageMaker's built\-in R kernel, how to get started with R on SageMaker, and finally several example notebooks\.

The examples are organized in three levels, Beginner, Intermediate, and Advanced\. They start from [Getting Started with R on SageMaker](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/r_examples/r_sagemaker_hello_world/r_sagemaker_hello_world.ipynb), continue to end\-to\-end machine learning with R on SageMaker, and then finish with more advanced topics such as SageMaker Processing with R script, and Bring\-Your\-Own \(BYO\) R algorithm to SageMaker\.  

## R Kernel in SageMaker<a name="w913aab7c27c27b7"></a>

 SageMaker notebook instances support R using a pre\-installed R kernel\. Also, the R kernel has the reticulate library, an R to Python interface, so you can use the features of SageMaker Python SDK from within an R script\. `paws` is an optional library that you can add to your R kernel to get further functionality\.  
+  [reticulatelibrary](https://rstudio.github.io/reticulate/): provides an R interface to the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. The reticulate package translates between R and Python objects\. 
+  [pawslibrary](https://cran.r-project.org/web/packages/paws/index.html): provides an R interface to make API calls to AWS services, similar to how `boto3` works\. `paws` enables Python developers to create, configure, and manage AWS services, such as EC2 and S3 using R\.  

## Get Started with R in SageMaker<a name="w913aab7c27c27b9"></a>
+   [Create a Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html) using the t2\.medium instance type and default storage size\. You can pick a faster instance and more storage if you plan to continue using the instance for more advanced examples, or create a bigger instance later\. 
+  Wait until the status of the notebook is In Service, and then click Open Jupyter\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/An-R-User-Guide-to-SageMaker/An-R-User-Guide-to-SageMaker-1.png) 
+  Create a new notebook with R kernel from the list of available environments\.  

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/An-R-User-Guide-to-SageMaker/An-R-User-Guide-to-SageMaker-2.png) 
+  When the new notebook is created, you should see an R logo in the upper right corner of the notebook environment, and also R as the kernel under that logo\. This indicates that SageMaker has successfully launched the R kernel for this notebook\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/An-R-User-Guide-to-SageMaker/An-R-User-Guide-to-SageMaker-3.png) 
+  Alternatively, when you are in a Jupyter notebook, you can use Kernel menu, and then select R from Change Kernel option\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/An-R-User-Guide-to-SageMaker/An-R-User-Guide-to-SageMaker-4.png) 

## Example Notebooks<a name="w913aab7c27c27c11"></a>

 **Prerequisites** 

 [Getting Started with R on SageMaker](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/r_examples/r_sagemaker_hello_world/r_sagemaker_hello_world.ipynb): This sample notebook describes how you can develop R scripts using Amazon SageMaker‘s R kernel\. In this notebook you set up your SageMaker environment and permissions, download the [abalone dataset](https://archive.ics.uci.edu/ml/datasets/abalone) from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php), do some basic processing and visualization on the data, then save the data as \.csv format to S3\. 

 **Beginner Level** 

 [End\-to\-End Machine Learning with R on SageMaker:](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/r_examples/r_end_2_end/r_sagemaker_abalone.ipynb) This sample notebook extends the previous prerequisite getting started notebook\. You learn how to train a model on the abalone dataset that predicts abalone age as measured by the number of rings in the shell\. After you train your model, you create an endpoint and deploy your model to the endpoint\. With your endpoint in place, you can test the model and generate predictions\. The reticulate package will be used as an R interface to the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.  

 [SageMaker Batch Transform using R Kernel](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/r_examples/r_batch_transform/r_xgboost_batch_transform.ipynb): This sample Notebook describes how to conduct a batch transform job using SageMaker’s Transformer API and the [XGBoost algorithm](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html)\. The notebook also uses the Abalone dataset\. 

 **Intermediate Level** 

 [Hyperparameter Optimization for XGBoost in R:](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/r_examples/r_xgboost_hpo_batch_transform/r_xgboost_hpo_batch_transform.ipynb) This sample notebook extends the previous beginner notebooks that use the abalone dataset and XGBoost\. It describes how to do model tuning with [hyperparamter optimization](https://sagemaker.readthedocs.io/en/stable/tuner.html)\. You will also learn how to use batch transform for batching predictions, as well as how to create a model endpoint to make real\-time predictions\.  

 [Amazon SageMaker Processing with R](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/r_examples/r_in_sagemaker_processing/r_in_sagemaker_processing.ipynb): [SageMaker Processing](https://aws.amazon.com/blogs/aws/amazon-sagemaker-processing-fully-managed-data-processing-and-model-evaluation/) lets you preprocess, post\-process and run model evaluation workloads\. This example shows you how to create an R script to orchestrate a Processing job\.  

 **Advanced Level** 

 [Train and Deploy Your Own R Algorithm in SageMaker:](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/r_examples/r_byo_r_algo_hpo) Do you already have an R algorithm, and you want to bring it into SageMaker to tune, train, or deploy it? This example walks you through how to customize SageMaker containers with custom R packages, all the way to using a hosted endpoint for inference on your R\-origin model\. 