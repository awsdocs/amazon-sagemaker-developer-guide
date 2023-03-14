# Retry Policy for Pipeline Steps<a name="pipelines-retry-policy"></a>

Retry policies help you automatically retry your SageMaker Pipelines steps after an error occurs\. Any pipeline step can encounter exceptions, and exceptions happen for various reasons\. In some cases, a retry can resolve these issues\. With a retry policy for pipeline steps, you can choose whether to retry a particular pipeline step or not\.

The retry policy only supports the following pipeline steps:
+ [Processing Step](build-and-manage-steps.md#step-type-processing) 
+ [Training Step](build-and-manage-steps.md#step-type-training) 
+ [Tuning Step](build-and-manage-steps.md#step-type-tuning) 
+ [AutoML Step](build-and-manage-steps.md#step-type-automl) 
+ [CreateModel Step](build-and-manage-steps.md#step-type-create-model) 
+ [RegisterModel Step](build-and-manage-steps.md#step-type-register-model) 
+ [Transform Step](build-and-manage-steps.md#step-type-transform) 

**Note**  
Jobs running inside both the tuning and AutoML steps conduct retries internally and will not retry the `SageMaker.JOB_INTERNAL_ERROR` exception type, even if a retry policy is configured\. You can program your own [ Retry Strategy](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RetryStrategy.html) using the SageMaker API\.



## Supported exception types for the retry policy<a name="pipelines-retry-policy-supported-exceptions"></a>

The retry policy for pipeline steps supports the following exception types:
+ `Step.SERVICE_FAULT`: These exceptions occur when an internal server error or transient error happens when calling downstream services\. SageMaker Pipelines retries on this type of error automatically\. With a retry policy, you can override the default retry operation for this exception type\.
+ `Step.THROTTLING`: Throttling exceptions can occur while calling the downstream services\. SageMaker Pipelines retries on this type of error automatically\. With a retry policy, you can override the default retry operation for this exception type\.
+ `SageMaker.JOB_INTERNAL_ERROR`: These exceptions occur when the SageMaker job returns `InternalServerError`\. In this case, starting a new job may fix a transient issue\.
+ `SageMaker.CAPACITY_ERROR`: The SageMaker job may encounter Amazon EC2 `InsufficientCapacityErrors`, which leads to the SageMaker job’s failure\. You can retry by starting a new SageMaker job to avoid the issue\. 
+ `SageMaker.RESOURCE_LIMIT`: You can exceeed the resource limit quota when running a SageMaker job\. You can wait and retry running the SageMaker job after a short period and see if resources are released\.

## The JSON schema for the retry policy<a name="pipelines-retry-policy-json-schema"></a>

The retry policy for Pipelines has the following JSON schema:

```
"RetryPolicy": {
   "ExceptionType": [String]
   "IntervalSeconds": Integer
   "BackoffRate": Double
   "MaxAttempts": Integer
   "ExpireAfterMin": Integer
}
```
+ `ExceptionType`: This field requires the following exception types in a string array format\.
  + `Step.SERVICE_FAULT`
  + `Step.THROTTLING`
  + `SageMaker.JOB_INTERNAL_ERROR`
  + `SageMaker.CAPACITY_ERROR`
  + `SageMaker.RESOURCE_LIMIT`
+ `IntervalSeconds` \(optional\): The number of seconds before the first retry attempt \(1 by default\)\. `IntervalSeconds` has a maximum value of 43200 seconds \(12 hours\)\.
+ `BackoffRate` \(optional\): The multiplier by which the retry interval increases during each attempt \(2\.0 by default\)\.
+ `MaxAttempts` \(optional\): A positive integer that represents the maximum number of retry attempts \(5 by default\)\. If the error recurs more times than `MaxAttempts` specifies, retries cease and normal error handling resumes\. A value of 0 specifies that errors are never retried\. `MaxAttempts` has a maximum value of 20\.
+ `ExpireAfterMin` \(optional\): A positive integer that represents the maximum timespan of retry\. If the error recurs after `ExpireAfterMin` minutes counting from the step gets executed, retries cease and normal error handling resumes\. A value of 0 specifies that errors are never retried\. `ExpireAfterMin ` has a maximum value of 14,400 minutes \(10 days\)\.
**Note**  
Only one of `MaxAttempts` or `ExpireAfterMin` can be given, but not both; if both are *not* specified, `MaxAttempts` becomes the default\. If both properties are identified within one policy, then the retry policy generates a validation error\.

## Configuring a retry policy<a name="pipelines-configuring-retry-policy"></a>

The following is an example of a training step with a retry policy\.

```
{
    "Steps": [
        {
            "Name": "MyTrainingStep",
            "Type": "Training",
            "RetryPolicies": [
                {
                    "ExceptionType": [
                        "SageMaker.JOB_INTERNAL_ERROR",
                        "SageMaker.CAPACITY_ERROR"
                    ],
                    "IntervalSeconds": 1,
                    "BackoffRate": 2,
                    "MaxAttempts": 5
                }
            ]
        }
    ]
}
```



The following is an example of how to build a `TrainingStep` in SDK for Python \(Boto3\) with a retry policy\.

```
from sagemaker.workflow.retry import (
    StepRetryPolicy, 
    StepExceptionTypeEnum,
    SageMakerJobExceptionTypeEnum,
    SageMakerJobStepRetryPolicy
)

step_train = TrainingStep(
    name="MyTrainingStep",
    xxx,
    retry_policies=[
        // override the default 
        StepRetryPolicy(
            exception_types=[
                StepExceptionTypeEnum.SERVICE_FAULT, 
                StepExceptionTypeEnum.THROTTLING
            ],
            expire_after_mins=5,
            interval_seconds=10,
            backoff_rate=2.0 
        ),
        // retry when resource limit quota gets exceeded
        SageMakerJobStepRetryPolicy(
            exception_types=[SageMakerJobExceptionTypeEnum.RESOURCE_LIMIT],
            expire_after_mins=120,
            interval_seconds=60,
            backoff_rate=2.0
        ),
        // retry when job failed due to transient error or EC2 ICE.
        SageMakerJobStepRetryPolicy(
            failure_reason_types=[
                SageMakerJobExceptionTypeEnum.INTERNAL_ERROR,
                SageMakerJobExceptionTypeEnum.CAPACITY_ERROR,
            ],
            max_attempts=10,
            interval_seconds=30,
            backoff_rate=2.0
        )
    ]
)
```

For more information on configuring retry behavior for certain step types, see *[Amazon SageMaker Model Building Pipelines \- Retry Policy](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_model_building_pipeline.html#retry-policy)* in the Amazon SageMaker Python SDK documentation\.