# Explore, Analyze, and Process Data<a name="how-it-works-notebooks-instances"></a>

Before using a dataset to train a model, data scientists typically explore, analyze, and preprocess it\.

To pre\-process data, use one of the following methods:
+ Use a Jupyter notebook on an Amazon SageMaker notebook instance to do the following:
  + Write code to create model training jobs
  + Deploy models to SageMaker hosting
  + Test or validate your models

  For a Get Started guide about training and deploying a model in the SageMaker Notebook insances, see [Get Started with Amazon SageMaker Notebook Instances](gs-console.md)\.

  For more information about the SageMaker Notebook insances, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. 
+  You can use a model to transform data by using SageMaker batch transform\. For more information, see [\(Optional\) Make Prediction with Batch Transform](ex1-model-deployment.md#ex1-batch-transform)\. 

Amazon SageMaker Processing enables running jobs to preprocess and postprocess data, perform feature engineering, and evaluate models on SageMaker easily and at scale\. When combined with the other critical machine learning tasks provided by SageMaker, such as training and hosting, Processing provides you with the benefits of a fully managed machine learning environment, including all the security and compliance support built into SageMaker\. With Processing, you have the flexibility to use the built\-in data processing containers or to bring your own containers and submit custom jobs to run on managed infrastructure\. After you submit a job, SageMaker launches the compute instances, processes and analyzes the input data, and releases the resources upon completion\. For more information, see [Process Data](processing-job.md)\.
+ For information about how to run your own data processing scripts, [Data Processing with scikit\-learn](use-scikit-learn-processing-container.md)\.
+ For information about how to build your own processing container to run scripts, see [Build Your Own Processing Container \(Advanced Scenario\)](build-your-own-processing-container.md)\.