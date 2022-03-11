# Data Parallel Troubleshooting<a name="distributed-troubleshooting-data-parallel"></a>

If you run into an error, you can use the following list to try to troubleshoot your training job\. If the problem persists, contact AWS Support\. 

**Topics**
+ [Using SageMaker Distributed Data Parallel with SageMaker Debugger and Checkpoints](#distributed-ts-data-parallel-debugger)
+ [An Unexpected Prefix Attached to Model Parameter Keys](#distributed-ts-data-parallel-pytorch-prefix)
+ [SageMaker Distributed Training Job Stalling During Initialization](#distributed-ts-data-parallel-efa-sg)
+ [SageMaker Distributed Training Job Stalling at the End of Training](#distributed-ts-data-parallel-stall-at-the-end)
+ [Lower Than Expected Scaling Efficiency Due to FSx Throughput Bottleneck](#distributed-ts-data-parallel-fsx-throughput-bottleneck)

## Using SageMaker Distributed Data Parallel with SageMaker Debugger and Checkpoints<a name="distributed-ts-data-parallel-debugger"></a>

To monitor system bottlenecks, profile framework operations, and debug model output tensors for training jobs with SageMaker distributed data parallel, use SageMaker Debugger\. 

However, when you use SageMaker Debugger, SageMaker distributed data parallel, and SageMaker checkpoints, you might see an error that looks like the following: 

```
SMDebug Does Not Currently Support Distributed Training Jobs With Checkpointing Enabled
```

This is due to an internal error between Debugger and checkpoints, which occurs when you enable SageMaker distributed data parallel\. 
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

## An Unexpected Prefix Attached to Model Parameter Keys<a name="distributed-ts-data-parallel-pytorch-prefix"></a>

For PyTorch distributed training jobs, an unexpected prefix \(`model` for example\) might be attached to `state_dict` keys \(model parameters\)\. The SageMaker data parallel library does not directly alter or prepend any model parameter names when PyTorch training jobs save model artifacts\. The PyTorch's distributed training changes the names in the `state_dict` to go over the network, prepending the prefix\. If you encounter any model failure problem due to different parameter names while you are using the SageMaker data parallel library and checkpointing for PyTorch training, adapt the following example code to remove the prefix at the step you load checkpoints in your training script:

```
state_dict = {k.partition('model.')[2]:state_dict[k] for k in state_dict.keys()}
```

This takes each `state_dict` key as a string value, separates the string at the first occurrence of `'model.'`, and takes the third list item \(with index 2\) of the partitioned string\.

For more information about the prefix issue, see a discussion thread at [Prefix parameter names in saved model if trained by multi\-GPU?](https://discuss.pytorch.org/t/prefix-parameter-names-in-saved-model-if-trained-by-multi-gpu/494) in the *PyTorch discussion forum*\.

For more information about the PyTorch methods for saving and loading models, see [Saving & Loading Model Across Devices](https://pytorch.org/tutorials/beginner/saving_loading_models.html#saving-loading-model-across-devices) in the *PyTorch documentation*\.

## SageMaker Distributed Training Job Stalling During Initialization<a name="distributed-ts-data-parallel-efa-sg"></a>

If your SageMaker distributed data parallel training job stalls during initialization when using EFA\-enabled instances \(`ml.p3dn.24xlarge` and `ml.p4d.24xlarge`\), this might be due to a misconfiguration in the security group of the VPC subnet that's used for the training job\. EFA requires a proper security group configuration to enable traffic between the nodes\.

**To configure inbound and outbound rules for the security group**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Choose **Security Groups** in the left navigation pane\.

1. Select the security group that's tied to the VPC subnet you use for training\. 

1. In the **Details** section, copy the **Security group ID**\.

1. On the **Inbound rules** tab, choose **Edit inbound rules**\.

1. On the **Edit inbound rules** page, do the following: 

   1. Choose **Add rule**\.

   1. For **Type**, choose **All traffic**\.

   1. For **Source**, choose **Custom**, paste the security group ID into the search box, and select the security group that pops up\.

1. Choose **Save rules** to finish configuring the inbound rule for the security group\.

1. On the **Outbound rules** tab, choose **Edit outbound rules**\.

1. Repeat the step 6 and 7 to add the same rule as an outbound rule\.

After you complete the preceding steps for configuring the security group with the inbound and outbound rules, rerun the training job and verify if the stalling issue is resolved\.

For more information about configuring security groups for VPC and EFA, see [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) and [Elastic Fabric Adapter](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/efa.html)\.

## SageMaker Distributed Training Job Stalling at the End of Training<a name="distributed-ts-data-parallel-stall-at-the-end"></a>

One of the root causes of stalling issues at the end of training is a mismatch in the number of batches that are processed per epoch across different ranks\. All workers \(GPUs\) synchronize their local gradients in the backward pass to ensure they all have the same copy of the model at the end of the batch iteration\. If the batch sizes are unevenly assigned to different worker groups during the final epoch of training, the training job stalls\. For example, while a group of workers \(group A\) finishes processing all batches and exits the training loop, another group of workers \(group B\) starts processing another batch and still expects communication from group A to synchronize the gradients\. This causes group B to wait for group A, which already completed training and does not have any gradients to synchronize\. 

Therefore, when setting up your train dataset, it is important that each worker gets the same number of data samples so that each worker goes through the same number of batches while training\. Make sure each rank gets the same number of batches to avoid this stalling issue\.

## Lower Than Expected Scaling Efficiency Due to FSx Throughput Bottleneck<a name="distributed-ts-data-parallel-fsx-throughput-bottleneck"></a>

One of the potential causes of lower than expected training scaling performance is the FSx throughput limit\. If you observe a sudden drop in scaling efficiency when switching to a larger cluster, try to use a larger FSx volume which has higher throughput and see if this increases performance\.
