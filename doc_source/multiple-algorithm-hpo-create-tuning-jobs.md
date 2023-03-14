# Create a Hyperparameter Optimization Tuning Job for One or More Algorithms \(Console\)<a name="multiple-algorithm-hpo-create-tuning-jobs"></a>

To create a new hyperparameter optimization \(HPO\) tuning job for one or more algorithms, you need to define the settings for the tuning job, create training job definitions for each algorithm being tuned, and configure the resources for the tuning job\.

**Topics**
+ [Define job settings](#multiple-algorithm-hpo-create-tuning-jobs-define-settings)
+ [Create Training Job Definitions](#multiple-algorithm-hpo-create-tuning-jobs-training-definitions)
+ [Configure Tuning Job Resources](#multiple-algorithm-hpo-resource-config)
+ [Review and Create HPO Tuning Job](#multiple-algorithm-hpo-review-and-create)

## Define job settings<a name="multiple-algorithm-hpo-create-tuning-jobs-define-settings"></a>

Your tuning job settings are applied across all of the algorithms in the HPO tuning job\. Warm start and early stopping are available only when tuning a single algorithm\. After you define the job settings you will create individual training definitions for each algorithm or variation you want to tune\. 

**Warm Start**  
If you cloned this job, you can choose to use the results from a previous tuning job to improve the performance of this new tuning job\. This is the warm start feature and it is only available when tuning a single algorithm\. When you choose this option, you can choose up to five previous hyperparameter tuning jobs to use\. Alternatively, you can use transfer learning to add additional data to the parent tuning job\. When you select this option, you choose one previous tuning job as the parent\. 

**Note**  
Warm start is compatible only with tuning jobs created after October 1, 2018\. For more information, see [Run a warm start job](automatic-model-tuning-considerations.html)\.

**Early Stopping**  
To reduce compute time and avoid overfitting your model, training jobs can be stopped early when they are unlikely to improve the current best objective metric of the hyperparameter tuning job\.\. Like warm start, this feature is only available when tuning a single algorithm\. This is an automatic feature without configuration options, and it’s disabled by default\. For more information on how early stopping works, the algorithms that support it, and how to use it with your own algorithms, see [Stop Training Jobs Early](automatic-model-tuning-early-stopping.html)\.

**Tuning Strategy**  
Tuning strategy can be either random, Bayesian or Hyperband\. These selections specify how automatic tuning algorithms search over specified hyperparameter ranges \(selected in a later step\)\. Random search chooses random combinations of values from the specified ranges and can be run sequentially or in parallel\. Bayesian optimization chooses values based on what is likely to get the best result given what is known about the history of previous selections\. Hyperband uses a multi\-fidelity strategy that dynamically allocates resources towards well\-utilized jobs and automatically stops those that underperform\. The new configuration that starts after stopping other configurations is chosen randomly\. Hyperband can only be used with iterative algorithms\. For more information search strategies, see [How Hyperparameter Tuning Works](automatic-model-tuning-how-it-works.html)\.

**Note**  
Hyperband uses an advanced internal mechanism to apply early stopping\. Thus, the parameter `TrainingJobEarlyStoppingType` in the `HyperParameterTuningJobConfig` API must be set to `OFF` when using Hyperband's internal early stopping feature\.

**Tags**  
You enter tags as key\-value pairs to assign metadata to tuning jobs to help you manage them\. Values are not required\. You can use just the key\. To see the keys associated with a job, choose the **Tags** tab on the details page for tuning job\. For more information about using tags for tuning jobs, see [Manage Hyperparameter Tuning and Training Jobs](multiple-algorithm-hpo-manage-tuning-jobs.md)

## Create Training Job Definitions<a name="multiple-algorithm-hpo-create-tuning-jobs-training-definitions"></a>

To create a training job definition, you need to configure the algorithm and parameters, define the data input and output, and configure resources\. You must provide at least one [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TrainingJobDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TrainingJobDefinition.html) for each HPO tuning job\. Each training definition specifies the configuration for an algorithm\. To create several definitions for your training job you can clone a job definition\. Cloning a job can save time as it copies all of the job settings, including data channels, S3 storage locations for output artifacts\. You can then edit the cloned job just for changes needed to configure the algorithm options\.

**Topics**
+ [Configure algorithm and parameters](#multiple-algorithm-hpo-algorithm-configuration)
+ [Define Data Input and Output](#multiple-algorithm-hpo-data)
+ [Configure Training Job Resources](#multiple-algorithm-hpo-training-job-definition-resources)
+ [Add or Clone a Training Job](#multiple-algorithm-hpo-add-training-job)

### Configure algorithm and parameters<a name="multiple-algorithm-hpo-algorithm-configuration"></a>

Each training job definition for a tuning job requires a name, permission to access services, and the specification of algorithm options, an objective metric, and the range of values, when required, to configure the set of hyperparameter values for each training job\. 

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
+ If you choose a built\-in algorithm, it has the ECR image information pre\-populated\.
+ If you choose your own container, you must specify the ECR image information\. You can select the input mode for the algorithm as file or pipe\.
+ If you plan to supply your data using a \.CSV file from Amazon S3, you should select the file\.

**Metrics**  
When you choose a built\-in algorithm, metrics are provided for you\. If you choose your own algorithm, you need to define your metrics\. You can define up to 20 metrics for your tuning job to monitor, one of which must be chosen as the objective metric\. For more information on how to define a metric for a tuning job, see [Define metricsSpecify environment variables](automatic-model-tuning-define-metrics-variables.md#automatic-model-tuning-define-metrics)\.

**Objective Metric**  
To find the best training job, set an objective metric and whether to maximize or minimize it\. After the training job is complete, you can view the tuning job detail page for a summary of the best training job found using this objective metric\. 

**Hyperparameter Configuration**  
When you choose a built\-in algorithm, the default values for its hyperparameters are set for you, using ranges that are optimized for the algorithm being tuned\. You can change these values as you see fit\. For example, instead of a range, you can set a fixed value for a hyperparameter by setting the parameter’s type to **static**\. Each algorithm has different required and optional parameters\. For more information, see [Best Practices for Hyperparameter Tuning](automatic-model-tuning-considerations.html) and [Define Hyperparameter Ranges](automatic-model-tuning-define-ranges.html)\. 

### Define Data Input and Output<a name="multiple-algorithm-hpo-data"></a>

Each training job definition for a tuning job must configures the channels for data inputs, data output locations, and optionally any checkpoint storage locations for each training job\. 

**Input Data Configuration**  
Input data is defined by channels, each with their own source location \(Amazon S3 or Amazon Elastic File System\), compression, and format options\. You can define up to 20 channels of input sources\. If the algorithm you chose supports multiple input channels, you can specify those too\. For example, when using the [XGBoost churn prediction notebook](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_applying_machine_learning/xgboost_customer_churn/xgboost_customer_churn.html), you could add two channels: train and validation\.

**Checkpoint Configuration**  
Checkpoints are periodically generated during training\. You must choose an Amazon S3 location for the checkpoints to be saved\. Checkpoints are used in metrics reporting, and are also used to resume managed spot training jobs\. For more information, see [Use Checkpoints in Amazon SageMaker](model-checkpoints.md)\.

**Output Data Configuration**  
You must define an Amazon S3 location for the artifacts of the training job to be stored\. You have the option of adding encryption to the output using an AWS Key Management Service \(AWS KMS\) key\. 

### Configure Training Job Resources<a name="multiple-algorithm-hpo-training-job-definition-resources"></a>

Each training job definition for a tuning job must configures the resources to deploy, including instance types and counts, managed spot training, and stopping conditions\.

**Resource Configuration**  
Each training definition can have a different resource configuration\. You choose the instance type and number of nodes\. 

**Managed spot training**  
You can save computer costs for jobs if you have flexibility in start and end times by allowing SageMaker to use spare capacity to run jobs\. For more information, see [Managed Spot Training in Amazon SageMaker](model-managed-spot-training.md)\.

**Stopping condition**  
The stopping condition specifies the maximum duration allowed per training job\. 

### Add or Clone a Training Job<a name="multiple-algorithm-hpo-add-training-job"></a>

Once you have created a training job definition for a tuning job, you are returned to the **Training Job Definition\(s\)** panel where you can create additional training job definitions to train additional algorithms\. You can select the **Add training job definition** and work through the steps to definite a training job again or choose **Clone** from the **Action** menu to replicate an existing training job definition and the edit it for the new algorithm\. The clone option can save time as it copies all of the job’s settings, including the data channels, S3 storage locations\. For more information on cloning, see [Manage Hyperparameter Tuning and Training Jobs](multiple-algorithm-hpo-manage-tuning-jobs.md)

## Configure Tuning Job Resources<a name="multiple-algorithm-hpo-resource-config"></a>

**Resource Limits**  
You can specify the maximum number of concurrent training jobs that a hyperparameter tuning job can run concurrently \(10 at most\) and the maximum number of training jobs that the hyperparameter tuning job can run \(500 at most\)\.The number of parallel jobs should not exceed the number of nodes you have requested across all of your training definitions\. The total number of jobs can’t exceed the number of jobs that your definitions are expected to run\.

## Review and Create HPO Tuning Job<a name="multiple-algorithm-hpo-review-and-create"></a>

Review the job settings, the training job definition\(s\), and resource limits\. Then select **Create hyperparameter tuning job**\.