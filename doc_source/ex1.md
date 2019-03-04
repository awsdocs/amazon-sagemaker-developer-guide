# Step 2: Train a Model with a Built\-in Algorithm and Deploy It<a name="ex1"></a>

Now train and deploy your first machine learning model with Amazon SageMaker\. For model training, you use the following: 
+ The MNIST dataset of images of handwritten, single digit numbers—This dataset provides 60,000 example images of handwritten single\-digit numbers and a test dataset of 10,000 images\. You provide this dataset to the k\-means algorithm for model training\. For more information, see [MNIST Dataset](http://www.deeplearning.net/tutorial/gettingstarted.html)\.
+ A built\-in algorithm—You use the k\-means algorithm provided by Amazon SageMaker\. K\-means is a clustering algorithm\. During model training, the algorithm groups the example data of handwritten numbers into 10 clusters \(one for each number, 0 through 9\)\. For more information about the algorithm, see [K\-Means Algorithm](k-means.md)\.

In this exercise, you do the following:

1. Download the MNIST dataset to your Amazon SageMaker notebook instance, then review the data and preprocess it\. For efficient training, you convert the dataset from the `numpy.array` format to the `RecordIO protobuf` format\. A `numpy.array` is an n\-dimensional array object that the NumPy scientific computing library uses\. `RecordIO protobuf` is a binary data format that the Amazon SageMaker K\-Means algorithm expects as input\.

1. Start an Amazon SageMaker training job\.

1. Deploy the model in Amazon SageMaker\.

1. Validate the model by sending inference requests to the model's endpoint\. You send images of handwritten, single\-digit numbers\. The model returns the number of the cluster \(0 through 9\) that the images belong to\. 

**Important**  
For model training, deployment, and validation, you can use either of the following:  
The high\-level Python library provided by Amazon SageMaker
The AWS SDK for Python \(Boto\)
The high\-level library abstracts several implementation details, and is easy to use\. This exercise provides separate code examples using both libraries\. If you're a first\-time Amazon SageMaker user, we recommend that you use the high\-level Python library\. For more information, see [The Amazon SageMaker Programming Model ](how-it-works-prog-model.md)\. 

There are two ways to use this exercise:
+ Follow the steps to create, deploy, and validate the model\. You create a Jupyter notebook in your Amazon SageMaker notebook instance, and copy code, paste it into the notebook, and run it\. 
+ If you're familiar with using sample notebooks, open and run the following example notebooks that Amazon SageMaker provides in the **SageMaker Python SDK** section of the **SageMaker Examples** tab of your notebook instance:
  + `kmeans_mnist.ipynb`
  + `kmeans_mnist_lowlevel.ipynb`

**Topics**
+ [Step 2\.1: Create a Jupyter Notebook and Initialize Variables](ex1-prepare.md)
+ [Step 2\.2: Download, Explore, and Transform the Training Data](ex1-preprocess-data.md)
+ [Step 2\.3: Train a Model](ex1-train-model.md)
+ [Step 2\.4: Deploy the Model to Amazon SageMaker](ex1-model-deployment.md)
+ [Step 2\.5: Validate the Model](ex1-test-model.md)