# Troubleshooting guide<a name="notebook-auto-run-troubleshoot"></a>

Refer to this troubleshooting guide to help you debug failures you might experience when your scheduled notebook job runs\.

## Job definition doesn’t create jobs<a name="notebook-auto-run-troubleshoot-no-jobs"></a>

If your job definition doesn’t initiate any jobs, see the following possible causes:

**Missing permissions**
+ The role assigned to the job definition does not have a trust relationship with Amazon EventBridge\. That is, EventBridge cannot assume the role\.
+ The role assigned to the job definition does not have permission to call `SageMaker:StartPipelineExecution`\.
+ The role assigned to the job definition does not have permission to call `SageMaker:CreateTrainingJob`\.

**EventBridge quota exceeded**

If you see a `Put*` error such as the following example, you exceeded an EventBridge quota\. To resolve this, you can clean up unused EventBridge runs, or ask AWS Support to increase your quota\.

```
LimitExceededException) when calling the PutRule operation: 
The requested resource exceeds the maximum number allowed
```

For more information about EventBridge quotas, see [Amazon EventBridge quotas](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-quota.html)\.

**Pipeline quota limit exceeded**

If you see an error such as the following example, you exceeded the number of pipelines that you can run\. To resolve this, you can clean up unused pipelines in your account, or ask AWS Support to increase your quota\.

```
ResourceLimitExceeded: The account-level service limit 
'Maximum number of pipelines allowed per account' is XXX Pipelines, 
with current utilization of XXX Pipelines and a request delta of 1 Pipelines.
```

For more information about pipeline quotas, see [Amazon SageMaker endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html)\.

**Training job limit exceeded**

If you see an error such as the following example, you exceeded the number of training jobs that you can run\. To resolve this, reduce the number of training jobs in your account, or ask AWS Support to increase your quota\.

```
ResourceLimitExceeded: The account-level service limit 
'ml.m5.2xlarge for training job usage' is 0 Instances, with current 
utilization of 0 Instances and a request delta of 1 Instances. 
Please contact AWS support to request an increase for this limit.
```

For more information about training job quotas, see [Amazon SageMaker endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html)\.