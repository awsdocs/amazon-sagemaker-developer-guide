# Resource Limits for Automatic Model Tuning<a name="automatic-model-tuning-limits"></a>

SageMaker sets the following default limits for resources used by automatic model tuning:
+ Number of parallel \(concurrent\) hyperparameter tuning jobs: 100
+ Number of hyperparameters that can be searched: 30
**Note**  
Every possible value in a categorical hyperparameter counts against this limit\.
+ Number of metrics defined per hyperparameter tuning job: 20
+ Number of parallel \(concurrent\) training jobs per hyperparameter tuning job: 10
**Note**  
This can be increased to 100 jobs\.
+ \[Bayesian search strategy\] Number of training jobs per hyperparameter tuning job: 750
+ \[Random search strategy\] Number of training jobs per hyperparameter tuning job: 750
**Note**  
This can be increased up to 10,000 jobs\.
+ Maximum run time for a hyperparameter tuning job: 30 days

When you plan hyperparameter tuning jobs, you also have to take into account the limits on training resources\. For information about the default resource limits for SageMaker training jobs, see [SageMaker Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_sagemaker)\. Every concurrent training instance on which all of your hyperparameter tuning jobs run counts against the total number of training instances allowed\. For example, if you run 10 concurrent hyperparameter tuning jobs, each of those hyperparameter tuning jobs runs 100 total training jobs and 20 concurrent training jobs\. Each of those training jobs runs on one **ml\.m4\.xlarge** instance\. The following limits apply: 
+ Number of concurrent hyperparameter tuning jobs: You don't need to increase the limit, because 10 tuning jobs is below the limit of 100\.
+ Number of training jobs per hyperparameter tuning job: You don't need to increase the limit, because 100 training jobs is below the limit of 750\.
+ Number of concurrent training jobs per hyperparameter tuning job: You need to request a limit increase to 20, because the default limit is 10\.
+ SageMaker training **ml\.m4\.xlarge** instances: You need to request a limit increase to 200, because you have 10 hyperparameter tuning jobs, each of which is running 20 concurrent training jobs\. The default limit is 20 instances\.
+ SageMaker training total instance count: You need to request a limit increase to 200, because you have 10 hyperparameter tuning jobs, each of which is running 20 concurrent training jobs\. The default limit is 20 instances\.

**To request a quota increase:**

1. Open the [AWS Support Center](https://console.aws.amazon.com/support/home#/) page, sign in if necessary, and then choose **Create case**\. 

1. On the **Create case** page, choose **Service limit increase**\.

1. On the **Case details** panel, select **SageMaker Automatic Model Tuning \[Hyperparameter Optimization\]** for the **Limit type** 

1. On the **Requests** panel for **Request 1**, select the **Region**, the resource **Limit** to increase and the **New Limit value** you are requesting\. Select **Add another request** if you have additional requests for quota increases\.  
![\[\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/hpo/hpo-quotas-service-linit-increase-request.PNG)

1. In the **Case description** panel, provide a description of your use case \.

1. In the **Contact options** panel, select your preferred **Contact methods** \(**Web**, **Chat** or **Phone**\) and then choose **Submit**\. 