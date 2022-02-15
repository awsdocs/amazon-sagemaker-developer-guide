# Model Parallel Troubleshooting<a name="distributed-troubleshooting-model-parallel"></a>

If you run into an error, you can use the following list to try to troubleshoot your training job\. If the problem persists, contact [AWS Support](http://aws.amazon.com/premiumsupport)\. 

**Topics**
+ [Considerations for Using SageMaker Debugger with SageMaker Distributed Model Parallel](#distributed-ts-model-parallel-debugger)
+ [Saving Checkpoints](#distributed-ts-model-parallel-checkpoints)
+ [Convergence Using Model Parallel and TensorFlow](#distributed-ts-model-parallel-tf-convergence)
+ [Stalling or Crashing Distributed Training Jobs](#distributed-ts-model-parallel-training-issues)
+ [NCCL error](#distributed-ts-model-parallel-nccl-error)

## Considerations for Using SageMaker Debugger with SageMaker Distributed Model Parallel<a name="distributed-ts-model-parallel-debugger"></a>

SageMaker Debugger is not available for SageMaker distributed model parallel\. Debugger is enabled by default for all SageMaker TensorFlow and PyTorch training jobs, and you might see an error that looks like the following: 

```
FileNotFoundError: [Errno 2] No such file or directory: '/opt/ml/checkpoints/metadata.json.sagemaker-uploading
```

To fix this issue, disable Debugger by passing `debugger_hook_config=False` when creating a framework `estimator` as shown in the following example\.

```
bucket=sagemaker.Session().default_bucket()
base_job_name="sagemaker-checkpoint-test"
checkpoint_in_bucket="checkpoints"

# The S3 URI to store the checkpoints
checkpoint_s3_bucket="s3://{}/{}/{}".format(bucket, base_job_name, checkpoint_in_bucket)

estimator = TensorFlow(
    ...

    distribution={"smdistributed": {"modelparallel": { "enabled": True }}},
    checkpoint_s3_uri=checkpoint_s3_bucket,
    checkpoint_local_path="/opt/ml/checkpoints",
    debugger_hook_config=False
)
```

## Saving Checkpoints<a name="distributed-ts-model-parallel-checkpoints"></a>

You might run into the following error when saving checkpoints of a large model on SageMaker: 

```
InternalServerError: We encountered an internal error. Please try again
```

This could be caused by a SageMaker limitation while uploading the local checkpoint to Amazon S3 during training\. To disable checkpointing in SageMaker, use the following example to explicitly upload the checkpoints\.

If you run into the preceding error, do not use `checkpoint_s3_uri` with the SageMaker `estimator` call\. While saving checkpoints for larger models, we recommend saving checkpoints to a custom directory and passing the same to the helper function \(as a `local_path` argument\)\.

```
import os

def aws_s3_sync(source, destination):
    """aws s3 sync in quiet mode and time profile"""
    import time, subprocess
    cmd = ["aws", "s3", "sync", "--quiet", source, destination]
    print(f"Syncing files from {source} to {destination}")
    start_time = time.time()
    p = subprocess.Popen(cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    p.wait()
    end_time = time.time()
    print("Time Taken to Sync: ", (end_time-start_time))
    return

def sync_local_checkpoints_to_s3(local_path="/opt/ml/checkpoints", s3_uri=os.path.dirname(os.path.dirname(os.getenv('SM_MODULE_DIR', '')))+'/checkpoints'):
    """ sample function to sync checkpoints from local path to s3 """

    import boto3
    #check if local path exists
    if not os.path.exists(local_path):
        raise RuntimeError("Provided local path {local_path} does not exist. Please check")

    #check if s3 bucket exists
    s3 = boto3.resource('s3')
    if not s3_uri.startswith("s3://"):
        raise ValueError(f"Provided s3 uri {s3_uri} is not valid.")

    s3_bucket = s3_uri.replace('s3://','').split('/')[0]
    print(f"S3 Bucket: {s3_bucket}")
    try:
        s3.meta.client.head_bucket(Bucket=s3_bucket)
    except Exception as e:
        raise e
    aws_s3_sync(local_path, s3_uri)
    return

def sync_s3_checkpoints_to_local(local_path="/opt/ml/checkpoints", s3_uri=os.path.dirname(os.path.dirname(os.getenv('SM_MODULE_DIR', '')))+'/checkpoints'):
    """ sample function to sync checkpoints from s3 to local path """

    import boto3
    #try to create local path if it does not exist
    if not os.path.exists(local_path):
        print(f"Provided local path {local_path} does not exist. Creating...")
        try:
            os.makedirs(local_path)
        except Exception as e:
            raise RuntimeError(f"Failed to create {local_path}")

    #check if s3 bucket exists
    s3 = boto3.resource('s3')
    if not s3_uri.startswith("s3://"):
        raise ValueError(f"Provided s3 uri {s3_uri} is not valid.")

    s3_bucket = s3_uri.replace('s3://','').split('/')[0]
    print(f"S3 Bucket: {s3_bucket}")
    try:
        s3.meta.client.head_bucket(Bucket=s3_bucket)
    except Exception as e:
        raise e
    aws_s3_sync(s3_uri, local_path)
    return
```

Usage of helper functions:

```
#base_s3_uri - user input s3 uri or save to model directory (default)
#curr_host - to save checkpoints of current host
#iteration - current step/epoch during which checkpoint is saved

# save checkpoints on every node using local_rank
if smp.local_rank() == 0:
    base_s3_uri = os.path.dirname(os.path.dirname(os.getenv('SM_MODULE_DIR', '')))
    curr_host = os.environ['SM_CURRENT_HOST']
    full_s3_uri = f'{base_s3_uri}/checkpoints/{curr_host}/{iteration}'
    sync_local_checkpoints_to_s3(local_path=checkpoint_dir, s3_uri=full_s3_uri)
```

## Convergence Using Model Parallel and TensorFlow<a name="distributed-ts-model-parallel-tf-convergence"></a>

When you use SageMaker multi\-node training with TensorFlow and distributed model parallel, the loss may not converge as expected because the order of training input files may be different on each node\. This may cause different ranks in the same model parallel group to work on different input files, causing inconsistencies\. To prevent this, ensure the input files are ordered the same way in all the ranks before they get converted to TensorFlow datasets\. One way to achieve this is to sort the input file names in the training script\.

## Stalling or Crashing Distributed Training Jobs<a name="distributed-ts-model-parallel-training-issues"></a>

If your training job has stalling, crashing, or not responding issues, read the following troubleshooting items to identify what's the cause of the issue\. If you need any further support, reach out to the SageMaker distributed training team through [AWS Support](http://aws.amazon.com/premiumsupport)\.
+  If you see **a distributed training job stalling at the NCCL initialization step**, consider the following: 
  + If you are using one of the EFA\-enabled instances \( `ml.p4d` or `ml.p3dn` instances\) with a custom VPC and its subnet, ensure that the security group used has inbound and outbound connections for all ports to and from the same SG\. You also generally need outbound connections to any IP as a separate rule \(for internet access\)\. To find instructions on how to add inbound and outbound rules for EFA communication, refer to [SageMaker Distributed Training Job Stalling During Initialization](distributed-troubleshooting-data-parallel.md#distributed-ts-data-parallel-efa-sg)\.
+ If you see a **distributed training job stalling when checkpointing** the full model, this might be because the `state_dict()` call on the model or optimizer was not made on all ranks with `rdp_rank()==0` \(when using tensor parallelism\) or `dp_rank()==0` \(when using only pipeline parallelism\)\. These ranks need to communicate to construct the checkpoint to be saved\. Similar stalling issues can also happen when checkpointing partial optimizer if `shard_optimizer_state` is enabled\. 

  For more information about checkpointing a model with model parallelism, see [General Instruction for Saving and Loading](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#general-instruction-for-saving-and-loading) and [Instructions for Checkpointing with Tensor Parallelism](model-parallel-extended-features-pytorch-saving-loading-checkpoints.md)\.
+ If the training job crashes with a **CUDA Out of Memory error**, this means that the distributed training configuration needs to be adjusted to fit the model on the GPU cluster\. For more information and best practices, see [Setting Up the Right Configuration for a Given Model](model-parallel-best-practices.md#model-parallel-best-practices-configuration)\.
+ If the training job crashes with an **uncorrectable [ECC error](https://docs.nvidia.com/deploy/a100-gpu-mem-error-mgmt/index.html)**, this means that one of the GPUs in the cluster has gone bad\. If you need technical support, share the job ARN with the AWS team and restart your training job from a checkpoint if possible\.
+ In rare cases, a job configuration that worked previously but is close to the limits of GPU memory might fail later with a different cluster due to a **CUDA Out of Memory error**\. This could be because some GPU has lower available memory than usual due to ECC errors\.
+ **Network timeout crash** might happen when running a multinode job which doesn’t use all GPUs in the node\. To get around this, use all GPUs on the node by ensuring that the `processes_per_host` parameter is set to the number of GPUs in each instance\. For example, this is `processes_per_host=8` for `ml.p3.16xlarge`, `ml.p3dn.24xlarge`, and `ml.p4d.24xlarge` instances\.
+ If you find that your training job takes a long time during the data downloading stage, make sure the Amazon S3 path you provided to `checkpoint_s3_uri` for the SageMaker `Estimator` class is unique for the current training job\. If this path is reused across multiple training jobs running simultaneously, all those checkpoints are uploaded and downloaded to the same Amazon S3 path and might significantly increase checkpoint loading time\.
+ Use FSx for Lustre when you deal with large data and models\.
  + If your dataset is large and fetching it takes a long time, we recommend keeping your dataset in [FSx for Lustre](http://aws.amazon.com/fsx/lustre/)\.
  + When training models are beyond 10 billion parameters, we recommend using FSx for Lustre for checkpointing\.
  + After you create a file system, make sure to wait for the status to become **available** before starting a training job using it\. 

## NCCL error<a name="distributed-ts-model-parallel-nccl-error"></a>

If you encountered the following error, it might be due to a process running out of GPU memory\.

```
NCCL error in: ../torch/lib/c10d/ProcessGroupNCCL.cpp:825, unhandled system error, NCCL version 2.7.8
ncclSystemError: System call (socket, malloc, munmap, etc) failed.
```

You can resolve this by reducing the batch size or `active_microbatches`\. If auto partitioning is not resulting in a well\-balanced partitioning, you might have to consider manual partitioning\. For more information, see [Pipeline parallelism across nodes](model-parallel-best-practices.md#model-parallel-best-practices-configuration-pipeline-across-nodes)\.