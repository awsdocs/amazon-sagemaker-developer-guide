# Train Using SageMaker Managed Warm Pools<a name="train-warm-pools"></a>

SageMaker Managed Warm Pools let you retain and reuse provisioned infrastructure after the completion of a training job to reduce latency for repetitive workloads, such as iterative experimentation or running many jobs consecutively\. Subsequent training jobs that match specified parameters run on the retained warm pool infrastructure, which speeds up start times by reducing the time spent provisioning resources\. 

**Important**  
SageMaker Managed Warm Pools are a billable resource\. For more information, see [Billing](#train-warm-pools-billing)\.

**Topics**
+ [How it works](#train-warm-pools-how-it-works)
+ [Warm pool resource limits](#train-warm-pools-resource-limits)
+ [How to use SageMaker Managed Warm Pools](#train-warm-pools-how-to-use)
+ [Considerations](#train-warm-pools-considerations)

## How it works<a name="train-warm-pools-how-it-works"></a>

To use SageMaker Managed Warm Pools and reduce latency between similar consecutive training jobs, create a training job that specifies a `KeepAlivePeriodInSeconds` value in its `ResourceConfig`\. This value represents the duration of time in seconds to retain configured resources in a warm pool for subsequent training jobs\. 

**Topics**
+ [Warm pool lifecycle](#train-warm-pools-lifecycle)
+ [Warm pool creation](#train-warm-pools-creation)
+ [Matching training jobs](#train-warm-pools-matching-criteria)
+ [Maximum warm pool duration](#train-warm-pools-maximum-duration)
+ [Billing](#train-warm-pools-billing)

### Warm pool lifecycle<a name="train-warm-pools-lifecycle"></a>

1. Create an initial training job with a `KeepAlivePeriodInSeconds` value greater than 0\. When you run this first training job, this “cold\-starts” a cluster with typical startup times\. 

1. When the first training job completes, the provisioned resources are kept alive in a warm pool for the period specified in the `KeepAlivePeriodInSeconds` value\. As long as the cluster is healthy and the warm pool is within the specified `KeepAlivePeriodInSeconds`, then the warm pool status is `Available`\. 

1. The warm pool stays `Available` until it either identifies a matching training job for reuse or it exceeds the specified `KeepAlivePeriodInSeconds` and is terminated\. The maximum length of time allowed for the `KeepAlivePeriodInSeconds` is 3600 seconds \(60 minutes\)\. If the warm pool status is `Terminated`, then this is the end of the warm pool lifecycle\.

1. If the warm pool identifies a second training job with matching specifications such as instance count or instance type, then the warm pool moves from the first training job to the second training job for reuse\. The status of the first training job warm pool becomes `Reused`\. This is the end of the warm pool lifecycle for the first training job\. 

1. The status of the second training job that reused the warm pool becomes `InUse`\. After the second training job completes, the warm pool is `Available` for the `KeepAlivePeriodInSeconds` duration specified in the second training job\. A warm pool can continue moving to subsequent matching training jobs for a maximum of 7 days\.

1. If the warm pool is no longer available to reuse, the warm pool status is `Terminated`\. Warm pools are no longer available if they are terminated by a user, for a patch update, or for exceeding the specified `KeepAlivePeriodInSeconds`\.

For more information on warm pool status options, see [WarmPoolStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_WarmPoolStatus.html) in the *Amazon SageMaker API Reference*\.

### Warm pool creation<a name="train-warm-pools-creation"></a>

If an initial training job successfully completes and has a `KeepAlivePeriodInSeconds` value greater than 0, this creates a warm pool\. If you stop a training job after a cluster is already launched, a warm pool is still retained\. If the training job fails due to an algorithm or client error, a warm pool is still retained\. If the training job fails for any other reason that might compromise the health of the cluster, then the warm pool is not created\. 

To verify successful warm pool creation, check the warm pool status of your training job\. If a warm pool successfully provisions, the warm pool status is `Available`\. If a warm pool fails to provision, the warm pool status is `Terminated`\.

### Matching training jobs<a name="train-warm-pools-matching-criteria"></a>

For a warm pool to persist, it must find a matching training job within the time specified in the `KeepAlivePeriodInSeconds` value\. The next training job is a match if the following values are identical: 
+ `RoleArn` 
+ `ResourceConfig` values:
  + `InstanceCount`
  + `InstanceType`
  + `VolumeKmsKeyId`
  + `VolumeSizeInGB`
+ `VpcConfig` values:
  + `SecurityGroupIds`
  + `Subnets`
+ `EnableInterContainerTrafficEncryption`
+ `EnableNetworkIsolation`

All of these values must be the same for a warm pool to move to a subsequent training job for reuse\.

### Maximum warm pool duration<a name="train-warm-pools-maximum-duration"></a>

The maximum `KeepAlivePeriodInSeconds` for a single training job is 3600 seconds \(60 minutes\) and the maximum length of time that a warm pool cluster can continue running consecutive training jobs is 7 days\. 

Each subsequent training job must also specify a `KeepAlivePeriodInSeconds` value\. When the warm pool moves to the next training job, it inherits the new `KeepAlivePeriodInSeconds` value specified in that training job’s `ResourceConfig`\. In this way, you can keep a warm pool moving from training job to training job for a maximum of 7 days\.

If no `KeepAlivePeriodInSeconds` is specified, then the warm pool spins down after the training job completes\.

### Billing<a name="train-warm-pools-billing"></a>

SageMaker Managed Warm Pools are a billable resource\. Retrieve the warm pool status for your training job to check the billable time for your warm pools\. You can check the warm pool status either through the [Using the Amazon SageMaker console](#train-warm-pools-how-to-use-sagemaker-console) or directly through the [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html) API command\. For more information, see [WarmPoolStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_WarmPoolStatus.html) in the *Amazon SageMaker API Reference*\.

## Warm pool resource limits<a name="train-warm-pools-resource-limits"></a>

To get started, you must first request a service limit increase for SageMaker Managed Warm Pools\. The default resource limit for warm pools is 0\.

If a training job is created with `KeepAlivePeriodInSeconds` specified, but you did not request a warm pool limit increase, then a warm pool is not retained after the completion of the training job\. A warm pool is only created if your warm pool limit has sufficient resources\. After a warm pool is created, the resources are released when they move to a matching training job or if the `KeepAlivePeriodInSeconds` expires \(if the warm pool status is `Reused` or `Terminated`\)\.

### Request a warm pool quota increase<a name="train-warm-pools-resource-limits-request"></a>

Request a warm pool quota increase using the AWS Service Quotas console\.

**Note**  
All warm pool instance usage counts toward your SageMaker training resource limit\. Increasing your warm pool resource limit does not increase your instance limit, but allocates a subset of your resource limit to warm pool training\.

1. Open the [AWS Service Quotas console](https://console.aws.amazon.com/servicequotas/home)\.

1. On the left\-hand navigation panel, choose **AWS services**\.

1. Search for and choose **Amazon SageMaker**\.

1. Search for the keyword **warm pool** to see all available warm pool service quotas\.

1. Find the instance type for which you want to increase your warm pool quota, select the warm pool service quota for that instance type, and choose **Request quota increase**\.

1. Enter your requested instance limit number under **Change quota value**\. The new value must be greater than the current **Applied quota value**\.

1. Choose **Request**\.

There is a limit on the number of instances that you can retain for each warm pool, which is determined by instance type\. You can check your resource limits in the [AWS Service Quotas console](https://console.aws.amazon.com/servicequotas/home) or directly using the [list\-service\-quotas](https://docs.aws.amazon.com/cli/latest/reference/service-quotas/list-service-quotas.html) AWS CLI command\. For more information on AWS Service Quotas, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\. 

You can also use [AWS Support Center](https://support.console.aws.amazon.com) to request a warm pool quota increase\. For a list of available instance types according to Region, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/) and choose **Training** in the **On\-Demand Pricing** table\.

## How to use SageMaker Managed Warm Pools<a name="train-warm-pools-how-to-use"></a>

You can use SageMaker Managed Warm Pools through the SageMaker Python SDK, the Amazon SageMaker console, or through the low\-level APIs\. Administrators can optionally use the `sagemaker:KeepAlivePeriod` condition key to further restrict the `KeepAlivePeriodInSeconds` limits for certain users or groups\.

**Topics**
+ [Using the SageMaker Python SDK](#train-warm-pools-how-to-use-python-sdk)
+ [Using the Amazon SageMaker console](#train-warm-pools-how-to-use-sagemaker-console)
+ [Using the low\-level SageMaker APIs](#train-warm-pools-how-to-use-low-level-apis)
+ [IAM condition key](#train-warm-pools-how-to-use-iam-condition-key)

### Using the SageMaker Python SDK<a name="train-warm-pools-how-to-use-python-sdk"></a>

Create, update, or terminate warm pools using the SageMaker Python SDK\.

**Note**  
This feature is available in the SageMaker [Python SDK v2\.110\.0](https://pypi.org/project/sagemaker/2.110.0/) and later\.

**Topics**
+ [Create a warm pool](#train-warm-pools-how-to-use-python-sdk-create)
+ [Update a warm pool](#train-warm-pools-how-to-use-python-sdk-update)
+ [Terminate a warm pool](#train-warm-pools-how-to-use-python-sdk-terminate)

#### Create a warm pool<a name="train-warm-pools-how-to-use-python-sdk-create"></a>

To create a warm pool, use the SageMaker Python SDK to create an estimator with a `keep_alive_period_in_seconds` value greater than 0 and call `fit()`\. When the training job completes, a warm pool is retained\. For more information on training scripts and estimators, see [Train a Model with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/overview.html#train-a-model-with-the-sagemaker-python-sdk)\. If your script does not create a warm pool, see [Warm pool creation](#train-warm-pools-creation) for possible explanations\.

```
import sagemaker
from sagemaker import get_execution_role
from sagemaker.tensorflow import TensorFlow

# Creates a SageMaker session and gets execution role
session = sagemaker.Session()
role = get_execution_role()

# Creates an example estimator
estimator = TensorFlow(
    ...
    entry_point='my-training-script.py',
    source_dir='code',
    role=role,
    model_dir='model_dir',
    framework_version='x.y.z',
    py_version='pyxy',
    job_name='my-training-job-1',
    instance_type='instance_type',
    instance_count=1,
    volume_size=250,
    hyperparameters={
        "batch-size": 512,
        "epochs": 1,
        "learning-rate": 1e-3,
        "beta_1": 0.9,
        "beta_2": 0.999,
    },
    keep_alive_period_in_seconds=1800,
)

# Starts a SageMaker training job and waits until completion
estimator.fit('s3://my_bucket/my_training_data/')
```

Next, create a second matching training job\. In this example, we create `my-training-job-2`, which has all of the necessary attributes to match with `my-training-job-1`, but has a different hyperparameter for experimentation\. The second training job reuses the warm pool and starts up faster than the first training job\. For more information on which attributes need to match, see [Matching training jobs](#train-warm-pools-matching-criteria)\.

```
# Creates an example estimator
estimator = TensorFlow(
    ...
    entry_point='my-training-script.py',
    source_dir='code',
    role=role,
    model_dir='model_dir',
    framework_version='x.y.z',
    py_version='pyxy',
    job_name='my-training-job-2',
    instance_type='instance_type',
    instance_count=1,
    volume_size=250,
    hyperparameters={
        "batch-size": 512,
        "epochs": 2,
        "learning-rate": 1e-3,
        "beta_1": 0.9,
        "beta_2": 0.999,
    },
    keep_alive_period_in_seconds=1800,
)

# Starts a SageMaker training job and waits until completion
estimator.fit('s3://my_bucket/my_training_data/')
```

Check the warm pool status of both training jobs to confirm that the warm pool is `Reused` for `my-training-job-1` and `InUse` for `my-training-job-2`\.

**Note**  
Training job names have date/time suffixes\. The example training job names `my-training-job-1` and `my-training-job-2` should be replaced with actual training job names\. You can use the `estimator.latest_training_job.job_name` command to fetch the actual training job name\.

```
session.describe_training_job('my-training-job-1')
session.describe_training_job('my-training-job-2')
```

The result of `describe_training_job` provides all details about a given training job\. Find the `WarmPoolStatus` attribute to check information about a training job’s warm pool\. Your output should look similar to the following example:

```
# Warm pool status for training-job-1
...
'WarmPoolStatus': {'Status': 'Reused', 
  'ResourceRetainedBillableTimeInSeconds': 1000,
  'ReusedByName': my-training-job-2}
...

# Warm pool status for training-job-2
... 
'WarmPoolStatus': {'Status': 'InUse'}
...
```

#### Update a warm pool<a name="train-warm-pools-how-to-use-python-sdk-update"></a>

When the training job is complete and the warm pool status is `Available`, then you can update the `KeepAlivePeriodInSeconds` value\.

```
session.update_training_job(job_name, resource_config={"KeepAlivePeriodInSeconds":3600})
```

#### Terminate a warm pool<a name="train-warm-pools-how-to-use-python-sdk-terminate"></a>

To manually terminate a warm pool, set the `KeepAlivePeriodInSeconds ` value to 0\.

```
session.update_training_job(job_name, resource_config={"KeepAlivePeriodInSeconds":0})
```

The warm pool automatically terminates when it exceeds the designated `KeepAlivePeriodInSeconds` value or if there is a patch update for the cluster\.

### Using the Amazon SageMaker console<a name="train-warm-pools-how-to-use-sagemaker-console"></a>

You cannot create or update a warm pool through the console, but you can use the console to check the warm pool status and billable time of specific training jobs\. You can also use the console to see which matching training job reused the warm pool\.

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/ec2/) and choose **Training jobs** from the navigation pane\. The warm pool status of each training job is visible in the **Warm pool status** column\.

1. Select a training job ID for more information\.

1. Scroll down to the **Warm pool status** section to find the warm pool status, the time left if the warm pool status is `Available`, the warm pool billable seconds, and the name of the training job that reused the warm pool if the warm pool status is `Reused`\.

### Using the low\-level SageMaker APIs<a name="train-warm-pools-how-to-use-low-level-apis"></a>

Use SageMaker Managed Warm Pools with either the SageMaker API or the AWS CLI\.

#### SageMaker API<a name="train-warm-pools-how-to-use-low-level-apis-sagemaker"></a>

Set up SageMaker Managed Warm Pools using the SageMaker API with the following commands:
+ [ CreateTrainingJob ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)
+ [ UpdateTrainingJob ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateTrainingJob.html)
+ [ ListTrainingJobs ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListTrainingJobs.html)
+ [ DescribeTrainingJob ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html)

#### AWS CLI<a name="train-warm-pools-how-to-use-low-level-apis-cli"></a>

Set up SageMaker Managed Warm Pools using the AWS CLI with the following commands:
+ [create\-training\-job](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/create-training-job.html)
+ [update\-training\-job](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/update-training-job.html)
+ [list\-training\-jobs](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/list-training-jobs.html)
+ [describe\-training\-job](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/describe-training-job.html)

### IAM condition key<a name="train-warm-pools-how-to-use-iam-condition-key"></a>

Administrators can optionally use the `sagemaker:KeepAlivePeriod` condition key to further restrict the `KeepAlivePeriodInSeconds` limits for certain users or groups\. SageMaker Managed Warm Pools are limited to a `KeepAlivePeriodInSeconds` value of 3600 seconds \(60 minutes\), but administrators can lower this limit if needed\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EnforceKeepAlivePeriodLimit",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob"
            ],
            "Resource": "*",
            "Condition": {
                "NumericLessThanIfExists": {
                    "sagemaker:KeepAlivePeriod": 1800
                }
            }
        }
    ]
}
```

For more information, see [Condition keys for Amazon SageMaker](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsagemaker.html#amazonsagemaker-policy-keys) in the *Service Authorization Reference*\.

## Considerations<a name="train-warm-pools-considerations"></a>

Consider the following items when using SageMaker Managed Warm Pools\.
+ SageMaker Managed Warm Pools cannot be used with heterogeneous cluster training\. 
+ SageMaker Managed Warm Pools cannot be used with spot instances\.
+ SageMaker Managed Warm Pools are limited to a `KeepAlivePeriodInSeconds` value of 3600 seconds \(60 minutes\)\.
+ If a warm pool continues to successfully match training jobs within the specified `KeepAlivePeriodInSeconds` value, the cluster can only continue running for a maximum of 7 days\.