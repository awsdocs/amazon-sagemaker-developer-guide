# Create an Amazon SageMaker Autopilot experiment<a name="autopilot-automate-model-development-create-experiment"></a>

To create an Amazon SageMaker Autopilot experiment, you need to name it, provide locations for the input and output data, specify the target data to predict, and optionally the type of machine learning problem to solve\. When you create an Amazon SageMaker Autopilot job as a pilot experiment, SageMaker analyzes your data and creates a notebook with candidate model definitions\. If you choose to run the complete experiment option, SageMaker also trains and tunes these models on your behalf\. You can view statistics while the experiment is running\. Afterwards, you can compare trials and delve into the details\.

1. Open Amazon SageMaker Studio and sign in\.

1. Choose the **Create Autopilot experiment** option from the **Build model automatically** box\.   
![\[Launch the Create Amazon SageMaker Autopilot experiment page.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot-build-model-automatically.png)

1. Enter information about the experiment in the **Job Settings** form:
   + **Experiment Name** – Must be unique to your account in the current AWS Region and contain a maximum of 63 alphanumeric characters\. Can include hyphens \(\-\), but not spaces\.  
![\[Specify the name of the experiment.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot-experiment-name.png)
   + **Input data location \(S3 bucket\)** – The S3 bucket that contains your input data\.   
![\[Specify the location of the input data.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot-input-data-name.png)
**Note**  
Must be an s3:// formatted URL where Amazon SageMaker has write permissions\. The S3 bucket must be in the current AWS Region and must be in CSV format and contain at least 500 rows\.
     + **S3 bucket name** – The bucket name must be unique across all existing bucket names in S3\.
     + **S3 object key prefix** – The filename of the object in the bucket, including the path to the object within the bucket\.
     + **S3 bucket location** – The concatenation of the S3 bucket name and S3 object key prefix\.
   + **Is your S3 input a manifest file?** – A manifest file includes metadata with your input data\. The metadata specifies the location of your data in Amazon S3 storage, how the data are formatted, and which attributes from the dataset to use when training your model\. You can use a manifest file as an alternative to preprocessing when you have labeled data being streamed in Pipe mode\.  
![\[Indicate whether the S3 input data is in a manifest file.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot-manifest-file.png)
   + **Target attribute name** – The name of the data column you want the model to target for predictions\.  
![\[Specify the name of the target variable to predict.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot-target-attribute-name.png)
   + **Output data location \(S3 bucket\)** – The S3 bucket where you want to store the output data\.  
![\[Specify the location of the output data.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot-output-data-name.png)
**Note**  
Must be an s3:// formatted URL where Amazon SageMaker has write permissions\. The S3 bucket must be in the current AWS Region\.
     + **S3 bucket name** – The bucket name must be unique across all existing bucket names in S3\.
     + **S3 object key prefix** – The filename of the object in the bucket, including the path to the object within the bucket\.
     + **S3 bucket location** – The concatenation of the S3 bucket name and S3 object key prefix\.
   + Select the machine learning problem type:  
![\[Specify the type of machine learning problem.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot-ml-problem-type.png)
     + Auto – SageMaker infers the problem type from the values of the attribute you want to predict\. In some cases, SageMaker is unable to infer accurately, in which case you must provide the value for the job to succeed\.
     + Binary classification – Binary classification is a type of supervised learning that assigns an individual to one of two predefined and mutually exclusive classes based on their attributes\. For example, a medical diagnosis for whether an individual has a disease or not based on the results of diagnostic tests\.
     + Regression – Regression estimates the values of a dependent target variable based on one or more other variables or attributes that are correlated with it\. For example, house prices based on features such as square footage and number of bathrooms\.
     + Multiclass classification – Multiclass classification is a type of supervised learning that assigns an individual to one of several classes based on their attributes\. For example, the prediction of the topic most relevant to a text document, such as politics, finance, or philosophy\.
   + Do you want to run a complete experiment?  
![\[Specify whether the experiment to be run to completion or to be a pilot.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot-experiment-pilot-or-complete.png)

     If you choose **Yes**, SageMaker generates a model as well as statistics that you can view in real time while the experiment is running\. After the experiment is complete, you can view the trials, sort by objective metric, and right\-click to deploy the model for use in other environments\.

     If you choose **No**, instead of executing the entire workflow, SageMaker stops execution after generating a notebook with candidate definitions\. A candidate is a combination of data preprocessors, algorithms, and algorithm parameter settings\. You can use the notebook as a starting point to guide your own process of model training/tuning\. The notebook has highlighted sections that explain what kinds of changes are typical, such as changing instance type, cluster size, and so on\.

1. Choose **Create Experiment**\.