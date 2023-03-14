# SageMaker Distributed Model Parallelism Best Practices<a name="model-parallel-best-practices"></a>

Use the following guidelines when you run a distributed training job with the SageMaker model parallel library\.

## Setting Up the Right Configuration for a Given Model<a name="model-parallel-best-practices-configuration"></a>

When scaling up a model, we recommend you to go over the following list in order\. Each list item discusses the advantage of using the library's techniques along with the tradeoffs that might arise\. 

**Tip**  
If a model can fit well using a subset of the library's features, adding more model parallelism or memory saving features does not usually improve performance\.

**Using large GPU instance types**
+ In the realm of model parallelism, it is best to use powerful instances with large GPU memories to handle overhead from model parallelism operations such as partitioning models across multiple GPUs\. We recommend using `ml.p4d` or `ml.p3dn` instances for training large DL models\. These instances are also equipped with Elastic Fabric Adapter \(EFA\), which provides higher network bandwidth and enables large\-scale training with model parallelism\.

**Sharding optimizer state**
+ The impact of sharding optimizer state depends on the number of data parallel ranks\. Typically, a higher degree of data parallelism \(proportional to the size of compute node\) can improve the efficiency of memory usage\.

  When you want to downsize a cluster, make sure you check the optimizer state sharding configuration\. For example, a large DL model with optimizer state sharding that fits on a node with 16 GPUs won't fit on a node with 8 GPUs because there are simply not enough GPUs across which to shard the optimizer state\.

  For more information, see [Optimizer State Sharding](model-parallel-extended-features-pytorch-optimizer-state-sharding.md)\.

**Activation checkpointing**
+ Memory efficiency can be improved by using activation checkpointing for a group of modules\. The more you group the modules, the more efficient the memory usage\. When checkpointing sequential modules for layers, the `strategy` argument of the `smp.set_activation_checkpointing` function groups the layers together for checkpointing\. For example, grouping two or more layers together for checkpointing is more memory efficient than checkpointing one layer at a time, and this trades extra computation time for reduced memory usage\.

  For more information, see [Activation Checkpointing](model-parallel-extended-features-pytorch-activation-checkpointing.md)\.

**Tensor parallelism**
+ The degree of tensor parallelism should be a power of two \(2, 4, 8, \.\.\., 2n\), where the maximum degree must be equal to the number of GPUs per node\. For example, if you use a node with 8 GPUs, possible numbers for the degree of tensor parallelism are 2, 4, and 8\. We don’t recommend arbitrary numbers \(such as 3, 5, 6, and 7\) for the degree of tensor parallelism\. When you use multiple nodes, misconfiguring the degree of tensor parallelism might result in running tensor parallelism across the nodes; this adds significant overhead from communication of activations across the nodes and can become computationally expensive\.

  For more information, see [Tensor Parallelism](model-parallel-extended-features-pytorch-tensor-parallelism.md)\.<a name="model-parallel-best-practices-configuration-pipeline-across-nodes"></a>

**Pipeline parallelism across nodes**
+ You can run pipeline parallelism both within a single node and across multiple nodes\. When you use pipeline parallelism in combination with tensor parallelism, we recommend running pipeline parallelism across multiple nodes and keeping tensor parallelism within individual nodes\. 
+ Pipeline parallelism comes with the following three knobs: `microbatches`, `active_microbatches`, and `prescaled_batch`\.
  + When you use tensor parallelism with pipeline parallelism, we recommend activating `prescaled_batch` so that the batch size per model parallel group can be increased for efficient pipelining\. With `prescaled_batch` activated, the batch size set in the training script becomes `tp_size` times the batch size set for each rank without `prescaled_batch`\.
  + Increasing the number of `microbatches` helps achieve efficient pipelining and better performance\. Note that the effective microbatch size is the batch size divided by number of microbatches\. If you increase the number of microbatches while keeping batch size constant, each microbatch processes fewer samples\.
  + The number of `active_microbatches` is the maximum number of microbatches that are simultaneously in process during pipelining\. For each active microbatch in process, its activations and gradients take up GPU memory\. Therefore, increasing `active_microbatches` takes up more GPU memory\.
+ If both GPU and GPU memory are underutilized, increase `active_microbatches` for better parallelization during pipelining\.
+ For more information about how to use tensor parallelism with pipeline parallelism, see [Tensor parallelism combined with pipeline parallelism](model-parallel-extended-features-pytorch-tensor-parallelism-examples.md#model-parallel-extended-features-pytorch-tensor-and-pipeline-parallelism)\.
+ To find descriptions of the aforementioned parameters, see [Parameters for `smdistributed`](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html#parameters-for-smdistributed) in the *SageMaker Python SDK documentation*\.

**Offloading activations to CPU**
+ Make sure that this is used in combination with activation checkpointing and pipeline parallelism\. To ensure that the offloading and preloading happen in the background, specify a value greater than 1 to the microbatches parameter\. 
+ When offloading activations, you might be able to increase `active_microbatches` and sometimes match with the total number of microbatches\. This depends on which modules are checkpointed and how the model is partitioned\.

  For more information, see [Activation Offloading](model-parallel-extended-features-pytorch-activation-offloading.md)\.

### Reference configurations<a name="model-parallel-best-practices-configuration-reference"></a>

The SageMaker model parallelism training team provides the following reference points based on experiments with the GPT\-2 model, the sequence length of 512, and the vocabulary size of 50,000\. 


| The number of model parameters | Instance type | Pipeline parallelism | Tensor parallelism | Optimizer state sharding | Activation checkpointing | Prescaled batch | Batch size | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 10 billion | 16 ml\.p4d\.24xlarge | 1 | 4 | True | Each transformer layer | True | batch\_size=40 | 
| 30 billion | 16 ml\.p4d\.24xlarge | 1 | 8 | True | Each transformer layer | True | batch\_size=32 | 
| 60 billion | 32 ml\.p4d\.24xlarge | 2 | 8 | True | Each transformer layer | True | batch\_size=56, microbatches=4, active\_microbatches=2 | 

You can extrapolate from the preceding configurations to estimate GPU memory usage for your model configuration\. For example, if you increase the sequence length for a 10\-billion\-parameter model or increase the size of the model to 20 billion, you might want to lower batch size first\. If the model still doesn’t fit, try increasing the degree of tensor parallelism\.

## Modifying Your Training Script<a name="model-parallel-best-practices-modify-training-script"></a>
+ Before you use the SageMaker model parallel library’s features in your training script, review [The SageMaker Distributed Model Parallelism Library Configuration Tips and Pitfalls](model-parallel-customize-tips-pitfalls.md)\.
+ To launch a training job faster, use the [SageMaker local mode](https://sagemaker.readthedocs.io/en/stable/overview.html?highlight=local%20mode#local-mode)\. This helps you quickly run a training job locally on a SageMaker notebook instance\. Depending on the scale of the ML instance on which your SageMaker notebook instance is running, you might need to adjust the size of your model by changing the model configurations, such as the hidden width, number of transformer layers, and attention heads\. Validate if the reduced model runs well on the notebook instance before using a large cluster for training the full model\. 

## Monitoring and Logging a Training Job Using the SageMaker Console and Amazon CloudWatch<a name="model-parallel-best-practices-monitoring"></a>

To monitor system\-level metrics such as CPU memory utilization, GPU memory utilization, and GPU utilization, use visualization provided through the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Training**\.

1. Choose **Training jobs**\.

1. In the main pane, choose the training job name for which you want to see more details\.

1. Browse the main pane and find the **Monitor** section to see the automated visualization\.

1. To see training job logs, choose **View logs** in the **Monitor** section\. You can access the distributed training job logs of the training job in CloudWatch\. If you launched multi\-node distributed training, you should see multiple log streams with tags in the format of **algo\-n\-1234567890**\. The **algo\-1** log stream tracks training logs from the main \(0th\) node\.

For more information, see [Monitor and Analyze Training Jobs Using Amazon CloudWatch Metrics](training-metrics.md)\.

## Permissions<a name="model-parallel-best-practices-permissions"></a>

To run a SageMaker training job with model parallelism or the [SageMaker distributed training example notebooks](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/index.html), make sure you have the right permissions in your IAM role, such as the following:
+ To use [FSx for Lustre](http://aws.amazon.com/fsx/), add [https://console.aws.amazon.com/iam/home#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonFSxFullAccess](https://console.aws.amazon.com/iam/home#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonFSxFullAccess)\.
+ To use Amazon S3 as a data channel, add [https://console.aws.amazon.com/iam/home#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonS3FullAccess](https://console.aws.amazon.com/iam/home#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonS3FullAccess)\.
+ To use Docker, build your own container, and push it to Amazon ECR, add [https://console.aws.amazon.com/iam/home#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonEC2ContainerRegistryFullAccess](https://console.aws.amazon.com/iam/home#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonEC2ContainerRegistryFullAccess)\.
+ To have a full access to use the entire suite of SageMaker features, add [https://console.aws.amazon.com/iam/home#/policies/iam/home#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home#/policies/iam/home#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonSageMakerFullAccess)\. 