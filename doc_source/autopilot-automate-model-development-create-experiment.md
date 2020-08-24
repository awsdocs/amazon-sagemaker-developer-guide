# Create an Amazon SageMaker Autopilot experiment<a name="autopilot-automate-model-development-create-experiment"></a>

When you create an Amazon SageMaker Autopilot experiment, Amazon SageMaker analyzes your data and creates a notebook with candidate model definitions\. If you choose to run the complete experiment, SageMaker trains and tunes these models on your behalf\. You can view statistics while the experiment is running\. Afterwards, you can compare trials and delve into the details\.

1. Open Amazon SageMaker Studio and sign in\.

1. On the left sidebar, choose the **SageMaker Experiment List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png)\)\.

1. Choose **Create Experiment**\.

1. Enter the experiment’s details in the **Job Settings** form:
   + Experiment Name – Must be unique to your account in the current AWS Region\.
   + Input data location \(S3 bucket\) – An s3:// formatted URL where SageMaker has read permissions\.
**Note**  
The input data must be in CSV format and contain at least 1000 rows\.
   + Target attribute name – The name of the data column you want the model to target\.
   + Output data location \(S3 bucket\) – An s3:// formatted URL where SageMaker has write permissions\.
**Note**  
The S3 bucket must be in the current AWS Region\.
   + Select the machine learning problem type:
     + Auto – SageMaker infers the problem type from the values of the attribute you want to predict\. In some cases, SageMaker is unable to infer accurately, in which case you must provide the value for the job to succeed\.
     + Binary classification – For example, dog or not dog\.
     + Regression – For example, house prices\.
     + Multiclass classification – For example, cat, dog, or bird\.
   + Do you want to run a complete experiment?

     If you choose **Yes**, SageMaker generates a model as well as statistics that you can view in real time while the experiment is running\. After the experiment is complete, you can view the trials, sort by objective metric, and right\-click to deploy the model for use in other environments\.

     If you choose **No**, instead of executing the entire workflow, SageMaker stops execution after generating a notebook with candidate definitions\. A candidate is a combination of data preprocessors, algorithms, and algorithm parameter settings\. You can use the notebook as a starting point to guide your own process of model training/tuning\. The notebook has highlighted sections that explain what kinds of changes are typical, such as changing instance type, cluster size, and so on\.

1. Choose **Create Experiment**\.