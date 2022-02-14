# Create an Amazon SageMaker Autopilot experiment<a name="autopilot-automate-model-development-create-experiment"></a>

When you create an Autopilot job as a pilot experiment, Autopilot analyzes your data and creates a notebook with candidate model definitions\. If you choose to run the complete experiment option, Autopilot also trains and tunes these models on your behalf\. You can view statistics while the experiment is running\. After it runs, you can compare trials and delve into the details\.

The following instructions show you how to create an Amazon SageMaker Autopilot experiment\. You will name it, provide locations for the input and output data, and specify which target data to predict\. Optionally, you can also specify the type of machine learning problem that you want to solve\. 

1. Open Amazon SageMaker Studio and sign in\. If you need information about launching Studio see [Use the Amazon SageMaker Studio Launcher](studio-launcher.md)\.

1. Choose the **New Autopilot experiment** option from the **Build model automatically** box\.   
![\[Launch the New Amazon SageMaker Autopilot experiment page.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/new-autopilot-experiment.PNG)

1. Enter information about the experiment in the **Basic settings** section of the **Create an Autopilot experiment** page:
   + **Experiment name** – Must be unique to your account in the current AWS Region and contain a maximum of 63 alphanumeric characters\. Can include hyphens \(\-\) but not spaces\.  
![\[Specify the name of the experiment.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/create-autopilot-experiment-name.PNG)
   + **Connect your data** – Provide the S3 bucket name and the dataset file name that contains your input data\.  
![\[S3 bucket name and Dataset file name fields in the Connect your data section of the Autopilot experiment page.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/create-autopilot-experiment-s3-bucket-input.PNG)
**Note**  
Must be an s3:// formatted URL where Amazon SageMaker has write permissions\. The S3 bucket must be in the current AWS Region and the file must be in CSV or parquet format and contain at least 5000 rows\.
     + **S3 bucket name** – The bucket name must be unique across all existing bucket names in Amazon S3\.
     + **S3 object key prefix** – The file name of the object in the bucket, including the path to the object within the bucket\.
     + **S3 bucket location** – The concatenation of the S3 bucket name and S3 object key prefix\.
   + **Is your S3 input a manifest file?** – A manifest file includes metadata with your input data\. The metadata specifies the location of your data in Amazon S3 storage, how the data is formatted, and which attributes from the dataset to use when training your model\. You can use a manifest file as an alternative to preprocessing when your labeled data is being streamed in Pipe mode\.  
![\[Toggle to indicate whether the S3 input data is in a manifest file.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/create-autopilot-experiment-manifest-file.PNG)
   + **Target** – The name of the data column that you want the model to target for predictions\.  
![\[Target field to specify the name of the target variable to predict.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/create-autopilot-experiment-target-column.PNG)
   + **Output data location \(S3 bucket\)** – The name of the S3 bucket and directory where you want to store the output data\.  
![\[Specify the location of the output data.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/create-autopilot-experiment-s3-bucket-output.PNG)
**Note**  
Must be an s3:// formatted URL where Amazon SageMaker has write permissions\. The S3 bucket must be in the current AWS Region\.
     + **S3 bucket name** – The bucket name must be unique across all existing bucket names in S3\.
     + **S3 object key prefix** – The file name of the object in the bucket, including the path to the object within the bucket\.
     + **S3 bucket location** – The concatenation of the S3 bucket name and S3 object key prefix\.

1. **Advanced settings \- *Optional*** – Autopilot provides additional controls that allow you to manually set experimental parameters\.
   + **Machine learning problem type** – Autopilot can automatically select the machine learning problem type\. If you prefer to specify it manually, use the **Select the machine learning problem type** dropdown menu\.  
![\[Specify the type of machine learning problem.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/create-autopilot-experiment-problem-type.PNG)
     + **Auto** – Autopilot infers the problem type from the values of the attribute that you want to predict\. In some cases, SageMaker is unable to infer accurately\. When that happens, you must provide the value for the job to succeed\.
     + **Binary classification **– Binary classification is a type of supervised learning that assigns an individual to one of two predefined, and mutually exclusive classes, based on their attributes\. For example, medical diagnosis based on results of diagnostic tests that determine if someone has a disease\.
     + **Regression** – Regression estimates the values of a dependent target variable based on one or more variables or attributes that are correlated with it\. For example, house prices based on features, such as square footage and number of bathrooms\.
     + **Multiclass classification** – Multiclass classification is a type of supervised learning that assigns an individual to one of several classes based on their attributes\. For example, the prediction of the topic most relevant to a text document, such as politics, finance, or philosophy\.
   + **Choose how to run your experiment** – You can specify how to run your experiment\.  
![\[Dropdown to choose whether to run a complete experiment or run a pilot.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/create-autopilot-experiment-advanced-options-run.PNG)

     If you choose **Yes**, Autopilot generates a model and statistics that you can view in real time while the experiment is running\. After the experiment is complete, you can view the trials, sort by objective metric, and deploy the model for use in other environments\. 

     If you choose **No**, instead of running the entire workflow, Autopilot stops running after generating a notebook with candidate definitions\. A candidate is a combination of data preprocessors, algorithms, and algorithm parameter settings\. You can use the notebook as a starting point to guide your own process of model training/tuning\. The notebook has highlighted sections that explain which changes are typical, such as changing instance type and cluster size\.
   + With additional advanced settings, you can specify runtime constraints and the IAM role to use for access\. You can also specify encryption keys and whether to use virtual private clouds \(VPCs\) for security, project labels, and tags\.  
![\[Specify additional advanced settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/create-autopilot-experiment-advanced-options.PNG)

     Information for each of these advanced settings is provided in the tooltips\.
   + To automatically deploy the best model from an Autopilot experiment to an endpoint, accept the default **Auto deploy** value **On** when creating the experiment\.  
![\[Select Auto deploy value set to on, which is the default value..\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-experiment-deploy.png)
**Note**  
Automatic deployment will fail if the default resource quota, or your customer quota for endpoint instances in a Region, is too limited\. Currently, you are required to have at least two ml\.m5\.2xlarge instances\. The eu\-north\-1 Region \(Stockholm\) does not meet this requirement\. The supported instance types for this Region are listed at [SageMaker Instance Types in EU \(Stockholm\) eu\-north\-1](http://aws.amazon.com/releasenotes/sagemaker-instance-types-in-stockholm-eu-north-1/)\. If you encounter this issue, you can request a service limit increase for SageMaker endpoint instances by following the  procedure at [Supported Regions and Quotas](regions-quotas.md)\. In the **Case details** panel, select **SageMaker Endpoints** for the **Limit type**\. For **Request1**, select:  
**Region**: **EU \(Stockholm\)**
**Resource Type**: **SageMaker Hosting**
**Limit**: **ml\.m5\.2xlarge** \(at least\)
**New limit value**: **2**
 

1. Select **Create experiment**\. Autopilot provides status on the course of the experiment\.  
![\[Status on creating the Amazon SageMaker Autopilot experiment.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/create-autopilot-experiment-status.PNG)

**Note**  
To avoid incurring unnecessary charges: If you deploy a model that is no longer needed, delete the endpoints and resources that were created during that deployment\. Information about pricing instances by Region is available at [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.