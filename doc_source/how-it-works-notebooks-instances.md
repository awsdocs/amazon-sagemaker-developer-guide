# Explore, Analyze, and Process Data<a name="how-it-works-notebooks-instances"></a>

Before using a dataset to train a model, data scientists typically explore, analyze, and preprocess it\. For example, in one of the exercises in this guide, you use the MNIST dataset, a commonly available dataset of handwritten numbers, for model training\. Before you begin training, you transform the data into a format that is more efficient for training\. For more information, see [Step 4\.3: Transform the Training Dataset and Upload It to Amazon S3](ex1-preprocess-data-transform.md)\. 

To preprocess data use one of the following methods:
+ Use a Jupyter notebook on a Amazon SageMaker notebook instance\. You can also use the notebook instance to do the following:
  + Write code to create model training jobs 
  + Deploy models to SageMaker hosting 
  + Test or validate your models

  For more information, see [Use Amazon SageMaker Notebook Instances](nbi.md) 
+  You can use a model to transform data by using SageMaker batch transform\. For more information, see [Step 6\.2: Deploy the Model with Batch Transform](ex1-batch-transform.md)\. 

Amazon SageMaker Processing enables running jobs to preprocess and postprocess data, perform feature engineering, and evaluate models on SageMaker easily and at scale\. When combined with the other critical machine learning tasks provided by SageMaker, such as training and hosting, Processing provides you with the benefits of a fully managed machine learning environment, including all the security and compliance support built into SageMaker\. With Processing, you have the flexibility to use the built\-in data processing containers or to bring your own containers and submit custom jobs to run on managed infrastructure\. After you submit a job, SageMaker launches the compute instances, processes and analyzes the input data, and releases the resources upon completion\. For more information, see [Process Data and Evaluate Models](processing-job.md)\.
+ For information about how to run your own data processing scripts, [Process Data and Evaluate Models with scikit\-learn](use-scikit-learn-processing-container.md)\.
+ For information about how to build your own processing container to run scripts, see [Build Your Own Processing Container \(Advanced Scenario\)](build-your-own-processing-container.md)\.