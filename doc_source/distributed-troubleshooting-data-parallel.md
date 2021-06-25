# Data Parallel Troubleshooting<a name="distributed-troubleshooting-data-parallel"></a>

If you run into an error, you can use the following list to try to troubleshoot your training job\. If the problem persists, contact AWS Support\. 

**Topics**
+ [Considerations for Using SageMaker Distributed Data Parallel with SageMaker Debugger and Checkpoints](#distributed-ts-data-parallel-debugger)
+ [An Unexpected Prefix \(`model` for example\) Is Attached to `state_dict` keys \(model parameters\) from a PyTorch Distributed Training Job](#distributed-ts-data-parallel-pytorch-prefix)

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

## An Unexpected Prefix \(`model` for example\) Is Attached to `state_dict` keys \(model parameters\) from a PyTorch Distributed Training Job<a name="distributed-ts-data-parallel-pytorch-prefix"></a>

The SageMaker data parallel library does not directly alter or prepend any model parameter names when PyTorch training jobs save model artifacts\. The PyTorch's distributed training changes the names in the `state_dict` to go over the network, prepending the prefix\. If you encounter any model failure problem due to the different parameter names while you are using the SageMaker data parallel library and checkpointing for PyTorch training, adapt the following example code to remove the prefix at the step you load checkpoints in your training script:

```
state_dict = {k.partition('model.')[2]:state_dict[k] for k in state_dict.keys()}
```

This takes each `state_dict` key as a string value, separates the string at the first occurrence of `'model.'`, and takes the third list item \(with index 2\) of the partitioned string\.

For more information about the prefix issue, see a discussion thread at [Prefix parameter names in saved model if trained by multi\-GPU?](https://discuss.pytorch.org/t/prefix-parameter-names-in-saved-model-if-trained-by-multi-gpu/494) in the *PyTorch discussion forum*\.

For more information about the PyTorch methods for saving and loading models, see [Saving & Loading Model Across Devices](https://pytorch.org/tutorials/beginner/saving_loading_models.html#saving-loading-model-across-devices) in the *PyTorch documentation*\.