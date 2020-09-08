# Get Started with Amazon SageMaker Notebook Instances and SDKs<a name="gs-console"></a>

The best way to learn how to use Amazon SageMaker is to create, train, and deploy a simple machine learning model\. To do this, you need the following:
+ A dataset\. You use the MNIST \(Modified National Institute of Standards and Technology database\) dataset of images of handwritten, single digit numbers\. This dataset provides a training set of 50,000 example images of handwritten single\-digit numbers, a validation set of 10,000 images, and a test dataset of 10,000 images\. You provide this dataset to the algorithm for model training\. For more information about the MNIST dataset, see [MNIST Dataset](http://yann.lecun.com/exdb/mnist/)\.
+ An algorithm\. You use the XGBoost algorithm provided by SageMaker to train the model using the MNIST dataset\. During model training, the algorithm assigns example data of handwritten numbers into 10 clusters: one for each number, 0 through 9\. For more information about the algorithm, see [XGBoost Algorithm](xgboost.md)\.

You also need a few resources for storing your data and running the code in this exercise:
+ An Amazon Simple Storage Service \(Amazon S3\) bucket to store the training data and the model artifacts that SageMaker creates when it trains the model\.
+ A SageMaker notebook instance to prepare and process data and to train and deploy a machine learning model\.
+ A Jupyter notebook to use with the notebook instance to prepare your training data and train and deploy the model\.

In this exercise, you learn how to create all of the resources that you need to create, train, and deploy a model\. 

**Important**  
For model training, deployment, and validation, you can use either of the following:  
The high\-level [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)
The AWS SDK for Python \(Boto3\)
The [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) abstracts several implementation details, and is easy to use\. This exercise provides code examples for both libraries\. If you're a first\-time SageMaker user, we recommend that you use the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

If you're new to SageMaker, we recommend that you read [How Amazon SageMaker Works](whatis.md#how-it-works) before starting this exercise\.

**Topics**
+ [Step 1: Create an Amazon S3 Bucket](gs-config-permissions.md)
+ [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)
+ [Step 3: Create a Jupyter Notebook](ex1-prepare.md)
+ [Step 4: Download, Explore, and Transform the Training Data](ex1-preprocess-data.md)
+ [Step 5: Train a Model](ex1-train-model.md)
+ [Step 6: Deploy the Model to Amazon SageMaker](ex1-model-deployment.md)
+ [Step 7: Validate the Model](ex1-test-model.md)
+ [Step 8: Integrating Amazon SageMaker Endpoints into Internet\-facing Applications](getting-started-client-app.md)
+ [Step 9: Clean Up](ex1-cleanup.md)