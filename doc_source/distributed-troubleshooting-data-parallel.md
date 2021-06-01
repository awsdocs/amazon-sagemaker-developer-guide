# Data Parallel Troubleshooting<a name="distributed-troubleshooting-data-parallel"></a>

If you run into an error, you can use the following list to try to troubleshoot your training job\. If the problem persists, contact AWS Support\. 

**Topics**
+ [Considerations for Using SageMaker Distributed Data Parallel with SageMaker Debugger and Checkpoints](#distributed-ts-data-parallel-debugger)

## Considerations for Using SageMaker Distributed Data Parallel with SageMaker Debugger and Checkpoints<a name="distributed-ts-data-parallel-debugger"></a>

To monitor system bottlenecks, profile framework operations, and debug model output tensors for training jobs with SageMaker distributed data parallel, you use SageMaker Debugger\. 

However, when you use SageMaker Debugger, SageMaker distributed data parallel, and SageMaker checkpoints, you might see an error that looks like the following: 

```
SMDebug Does Not Currently Support Distributed Training Jobs With Checkpointing Enabled
```

This is due to an internal error between Debugger and checkpoints, which occurs when you enable SageMaker Distributed\. 
+ If you enable all three features, SageMaker Python SDK automatically turns off Debugger by passing `debugger_hook_config=False`, which is equivalent to the following framework `estimator` example\.

  ```
  bucket=sagemaker.Session().default_bucket()
  base_job_name="sagemaker-checkpoint-test"
  checkpoint_in_bucket="checkpoints"
  
  # The S3 URI to store the checkpoints
  checkpoint_s3_bucket="s3://{}/{}/{}".format(bucket, base_job_name, checkpoint_in_bucket)
  
  estimator = TensorFlow(
      ...
      
      distribution={"smdistributed": {"dataparallel": { "enabled": True }}},
      checkpoint_s3_uri=checkpoint_s3_bucket,
      checkpoint_local_path="/opt/ml/checkpoints",
      debugger_hook_config=False
  )
  ```
+ If you want to keep using both SageMaker distributed data parallel and SageMaker Debugger, a workaround is manually adding checkpointing functions to your training script instead of specifying the `checkpoint_s3_uri` and `checkpoint_local_path` parameters from the estimator\. For more information about setting up manual checkpointing in a training script, see [Saving Checkpoints](distributed-troubleshooting-model-parallel.md#distributed-ts-model-parallel-checkpoints)\.