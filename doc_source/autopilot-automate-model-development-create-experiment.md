# Create an Amazon SageMaker experiment in SageMaker Studio<a name="autopilot-automate-model-development-create-experiment"></a>

1. Open Amazon SageMaker Studio and login\. 

1. Choose the **Experiments** tab \(it looks like a conical flask\)\. 

1. Choose **Create experiment**\. 

1. Enter the experiment’s details in the **Job Settings** form\. 
   + Name of the experiment \- Must be unique to this experiment in the current AWS Region\. 
   + Input dataset S3 location \- An s3:// formatted URL where SageMaker has read permissions\.
**Note**  
The input data must be in CSV format and contain at least 1000 rows\.
   + Target attribute \- This is the column of your data you want the model to target\. 
   + Output S3 location \- An s3:// formatted URL where SageMaker has write permissions\. 
   + \(Optional\) Tell Autopilot whether you want a regression \(e\.g\., house prices\), binary classification \(e\.g\., hotdog or not hotdog\), or multi\-class classification \(e\.g\., cat, dog or bird\) model\. If you don’t specify this, Autopilot infers it from the values of the attribute you want to predict\. In some cases, Autopilot is unable to infer accurately, in which case you must provide the value for the job to succeed\. 
   + \(Optional\) Tell Autopilot the metric you want it to use to evaluate models\. If you don’t specify a metric, Autopilot makes the decision on your behalf based on the best metrics for the situation\. 
   + \(Optional\) Configure other job parameters\. For example, security configuration or job completion criteria\. In the latter case, if you are looking to limit the total wall clock time of the AutoML job, you can specify to limit the number of seconds/minutes the job is allowed to run\. 
   + \(Optional\) Specify [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-GenerateCandidateDefinitionsOnly](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-GenerateCandidateDefinitionsOnly)\. In this case, instead of executing the entire AutoML workflow on Autopilot, Autopilot stops execution after generating the notebooks for data exploration and candidate generation\. This way, you can use the notebooks as a starting point to guide your own process of data exploration and model training/tuning\. Both notebooks have highlighted sections that explain what kinds of changes are typical, such as changing instance type, cluster size, and so on\. 

1. Choose to run the training immediately, or only produce data exploration and candidate generation notebooks\. 

This is all you need to run a Autopilot experiment\. The process will generate a model as well as statistics that you can view in real time while the experiment is running\. After the experiment is completed, you can view the trials, sort by objective metric, and right\-click to deploy the model for use in other environments\. 