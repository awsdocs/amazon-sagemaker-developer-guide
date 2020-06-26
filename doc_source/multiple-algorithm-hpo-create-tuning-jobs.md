# Create a new single or multi\-algorithm HPO tuning job<a name="multiple-algorithm-hpo-create-tuning-jobs"></a>

## Defining job settings<a name="multiple-algorithm-hpo-create-tuning-jobs-define-settings"></a>

 Your tuning job settings are applied across all of the algorithms in the HPO tuning job\. Warm start and early stopping are available only when tuning a single algorithm\. After you define the job settings you will create individual training definitions for each algorithm or variation you want to tune\. 

**Warm Start**  
 If you cloned this job, you can choose to use the results from a previous tuning job to improve the performance of this tuning job\. This is the warm start feature and it is only available when tuning a single algorithm\. When you choose this option, you can choose up to five previous hyperparameter tuning jobs to use\. Alternatively, you can use transfer learning to add additional data to the parent tuning job\. When you select this option, you choose one previous tuning job as the parent\. 

 Warm start is compatible with tuning jobs created after October 1, 2018\. For more information, see [Run a warm start job](automatic-model-tuning-considerations.html)\. 

**Early Stopping**  
 [Early stopping stops training jobs](automatic-model-tuning-early-stopping.html) when they are unlikely to improve the current best objective metric of the hyperparameter tuning job\. Like warm start, this feature is only available when tuning a single algorithm\. This is an automatic feature without configuration options, and it’s disabled by default\.  

**Tuning Strategy**  
 Tuning strategy can be either random or bayesian\. It specifies how the automatic tuning searches over specified hyperparameter ranges\. You specify the ranges in a later step\. For more information, see [How Hyperparameter Tuning Works](automatic-model-tuning-how-it-works.html)\. 

## Training Definitions<a name="multiple-algorithm-hpo-create-tuning-jobs-training-definitions"></a>

 You must provide at least one training definition for each training job\. Each training definition specifies the configuration for an algorithm\. To create several definitions for your training job you can clone a definition\. 

**Name**  
 Provide a unique name for the training definition\. 

**Permissions**  
 Amazon SageMaker requires permissions to call other services on your behalf\. Choose an IAM role or let AWS create a role that has the `AmazonSageMakerFullAccess` IAM policy attached\. 

**Optional Security Settings**  
 The network isolation setting prevents the container from making any outbound network calls\. This is required for AWS Marketplace machine learning offerings\. 

 You can also choose to use a private VPC\. 

**Note**  
 Inter\-container encryption is only available when creating job definitions from the API\. 

**Algorithm Options**  
 You can choose one of the built\-in algorithms, your own algorithm, your own container with an algorithm, or you can subscribe to an algorithm from AWS Marketplace\. 

 If you choose a built\-in algorithm, it has the ECR image information prepopulated\. If you choose your own container, you must specify the ECR image information\. You can select the input mode for the algorithm as file or pipe\. If you plan to supply your data using a \.CSV file from Amazon S3, you should select the file\. 

**Metrics**  
 When you choose a built\-in algorithm, metrics are provided for you\. If you choose your own algorithm, you need to define your metrics\. 

**Objective Metric**  
 To find the best training job, set an objective metric and tuning type\. After the training job is complete, you can view the tuning job detail page for a summary of the best training job found using this objective metric\. 

**Hyperparameter Configuration**  
 When you choose a built\-in algorithm, hyperparameters’ default values are set for you, using ranges that are optimized for the particular algorithm\. You can change these values as you see fit\. Instead of a range, you can set a fixed value for a hyperparameter by setting the parameter’s type to **static**\. Each algorithm has different required and optional parameters\. For more information, see [best practices](automatic-model-tuning-considerations.html) and [ranges](automatic-model-tuning-define-ranges.html)\. 

**Input Data Configuration**  
 Input data is defined by channels, each with their own source location \(Amazon S3 or Amazon Elastic File System\), compression, and format options\. You can define up to 20 channels of input sources\. If the algorithm you chose supports multiple input channels, you can specify those too\. 

 For example, when using the [XGBoost churn prediction notebook](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_applying_machine_learning/xgboost_customer_churn/xgboost_customer_churn.ipynb), you could add two channels: train and validation\. 

**Checkpoint Configuration**  
 Checkpoints are periodically generated during training\. You must choose an Amazon S3 location for the checkpoints to be saved\. Checkpoints are used in metrics reporting, and are also used to resume managed spot training jobs\. 

**Output Data Configuration**  
 You must define an Amazon S3 location for the artifacts of the training job to be stored\. You have the option of adding encryption to the output using an AWS Key Management Service \(AWS KMS\) key\. 

**Resource Limits and Configuration**  
 Each training definition can have a different resource configuration\. You choose the instance type and number of nodes\. 

**Finalizing the Job Settings**  
 You can run parallel jobs and limit the total number of jobs\. The number of parallel jobs should not exceed the number of nodes you have requested across all of your training definitions\. The total number of jobs can’t exceed the number of jobs that your definitions are expected to run\. 