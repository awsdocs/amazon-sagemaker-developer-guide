# Using an Amazon SageMaker Notebook Instance to Explore and Preprocess Data<a name="how-it-works-notebooks-instances"></a>

Before using a dataset to train a model, data scientists typically explore and preprocess it\. For example, in one of the exercises in this guide, you use the MNIST dataset, a commonly available dataset of handwritten numbers, for model training\. Before you begin training, you transform the data into a format that is more efficient for training\. For more information, see [Step 3\.2\.3: Transform the Training Dataset and Upload It to S3](ex1-preprocess-data-transform.md)\. 

To preprocess data, use a Jupyter notebook on an Amazon SageMaker notebook instance\. You can also use the notebook instance to write code to create model training jobs, deploy models to Amazon SageMaker hosting, and test or validate your models\. 

## Creating a Notebook Instance<a name="howitworks-create-ws"></a>

An *Amazon SageMaker notebook instance* is a fully managed ML compute instance running the Jupyter Notebook App\. Amazon SageMaker manages creating the instance and related resources\. 

To create a notebook instance, use either the Amazon SageMaker console or the [CreateNotebookInstance](API_CreateNotebookInstance.md) API\. In your request, provide a subnet and one or more security groups\.

After receiving the request, Amazon SageMaker does the following:

+ **Creates a network interface**—If you choose the optional VPC configuration, it creates the network interface in your VPC\. It uses the subnet ID that you provide in the request to determine which Availability Zone to create the subnet in\. Amazon SageMaker associates the security group that you provide in the request with the subnet\. For more information, see [ Notebook Instance Security](appendix-additional-considerations.md)\. 

+ **Launches an ML Compute instance**—Amazon SageMaker launches an ML compute instance in an Amazon SageMaker VPC\. It performs the configuration tasks that allow it to manage your notebook instance and if you specified your VPC, enables traffic between your VPC and the notebook instance\.

+ **Installs Anaconda packages and libraries for common deep learning platforms**—Installs all of the Anaconda packages that are included in the installer\. For more information, see [Anaconda package list](https://docs.anaconda.com/anaconda/packages/pkg-docs)\. In addition, Amazon SageMaker installs the TensorFlow and Apache MXNet deep learning libraries\. 

+ **Attaches An ML storage volume**— Attaches a 5 GB ML storage to the ML Compute instance\. You can use the volume to clean up the training dataset or to temporarily store other data to work with\.

+ **Copies example Jupyter notebooks**— These Python code examples illustrate model training and hosting exercises using various algorithms and training datasets\.

## Accessing Notebook Instances<a name="howitworks-access-ws"></a>

To access your Amazon SageMaker notebook instances, choose one of the following options: 

+ Use the console—Open a browser and sign in to the Amazon SageMaker console\. The console displays a list of notebook instances in your account\. To open a notebook instance, choose the **Open** action for the instance\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ws-notebook-10.png)

  The console uses your sign\-in credentials to send a [CreatePresignedNotebookInstanceUrl](API_CreatePresignedNotebookInstanceUrl.md) API request to Amazon SageMaker\. Amazon SageMaker returns the URL for your notebook instance, and the console opens the URL in another browser tab and displays the Jupyter notebook dashboard\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ws-notebook-20.png)

  Use the Jupyter notebook dashboard to create and manage notebooks and write code\. For more information about Jupyter notebooks, see [http://jupyter\.org/documentation\.html](http://jupyter.org/documentation.html)\.Note the following in the dashboard:

  + Under the **Files** tab, there is a `sample_notebook` folder\. It contains a set of example notebooks that Amazon SageMaker provides\. For more information about these examples, see [GitHub repository](https://github.com/awslabs/amazon-sagemaker-examples)\.

  + Customers can select a kernel for their notebook using the **New** drop\-down\. Amazon SageMaker provides several kernels for Jupyter including support for Python 2 and 3, MXNet, TensorFlow, and PySpark\.

+ Use the API—To get the URL for the notebook instance, call the [CreatePresignedNotebookInstanceUrl](API_CreatePresignedNotebookInstanceUrl.md) API\. 

## How It Works: Next Topic<a name="howitwork-ws-notebook-nextstep"></a>

 [Validating Machine Learning Models](how-it-works-model-validation.md) 