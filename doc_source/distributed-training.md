# Amazon SageMaker Distributed Training Libraries<a name="distributed-training"></a>

SageMaker provides distributed training libraries for data parallelism and model parallelism\. The libraries are optimized for the SageMaker training environment, help adapt your distributed training jobs to SageMaker, and improve training speed and throughput\.

**Topics**
+ [Get Started with Distributed Training](#distributed-training-get-started)
+ [Basic Distributed Training Concepts](#distributed-training-basic-concepts)
+ [Advanced Concepts](#distributed-training-advanced-concepts)
+ [Strategies](#distributed-training-strategies)
+ [Optimize Distributed Training](#distributed-training-optimize)
+ [Scenarios](#distributed-training-scenarios)
+ [SageMaker Built\-In Distributed Training Features](#distributed-training-built-in)
+ [SageMaker's Distributed Data Parallel Library](data-parallel.md)
+ [SageMaker's Distributed Model Parallel](model-parallel.md)
+ [Amazon SageMaker Distributed Training Notebook Examples](distributed-training-notebook-examples.md)

## Get Started with Distributed Training<a name="distributed-training-get-started"></a>

If you’re familiar with distributed training, follow one of the links to your preferred strategy or framework to get started\. Otherwise, continue on to the next section to learn some distributed training concepts\. 

 **SageMaker distributed training libraries:** 
+  [SageMaker's Distributed Data Parallel Library](data-parallel.md) 
+  [SageMaker's Distributed Model Parallel](model-parallel.md) 

**Note**  
The SageMaker distributed training libraries are available only through the AWS deep learning containers for the TensorFlow, PyTorch, and HuggingFace frameworks within the SageMaker training platform\. To use the libraries, you must use the SageMaker Python SDK or the SageMaker APIs through SDK for Python \(Boto3\) or AWS Command Line Interface\. Throughout the documentation, instructions and examples focus on how to use the distributed training libraries with the SageMaker Python SDK\.

## Basic Distributed Training Concepts<a name="distributed-training-basic-concepts"></a>

 SageMaker’s distributed training libraries use the following distributed training terms and features\. 

**Datasets and Batches**
+ **Training Dataset**: All of the data you use to train the model\.
+ **Global batch size**: The number of records selected from the training dataset in each iteration to send to the GPUs in the cluster\. This is the number of records over which the gradient is computed at each iteration\. If data parallelism is used, it is equal to the total number of model replicas multiplied by the per\-replica batch size: `global batch size = (the number of model replicas) * (per-replica batch size)`\. A single batch of global batch size is often referred to as the *mini\-batch * in machine learning literature\.
+ **Per\-replica batch size:** When data parallelism is used, this is the number of records sent to each model replica\. Each model replica performs a forward and backward pass with this batch to calculate weight updates\. The resulting weight updates are synchronized \(averaged\) across all replicas before the next set of per\-replica batches are processed\. 
+ **Micro\-batch**: A subset of the mini\-batch or, if hybrid model and data parallelism is used , it is a subset of the per\-replica sized batch \. When you use SageMaker’s distributed model parallelism library, each micro\-batch is fed into the training pipeline one\-by\-one and follows an [execution schedule](https://docs.aws.amazon.com/sagemaker/latest/dg/model-parallel-core-features.html#model-parallel-pipeline-execution) defined by the library's runtime\.

**Training**
+ **Epoch**: One training cycle through the entire dataset\. It is common to have multiple iterations per an epoch\. The number of epochs you use in training is unique on your model and use case\.
+ **Iteration**: A single forward and backward pass performed using a global batch sized batch \(a mini\-batch\) of training data\. The number of iterations performed during training is determined by the global batch size and the number of epochs used for training\. For example, if a dataset includes 5,000 samples, and you use a global batch size of 500, it will take 10 iterations to complete a single epoch\.
+ **Learning rate**: A variable that influences the amount that weights are changed in response to the calculated error of the model\. The learning rate plays an important role in the model’s ability to converge as well as the speed and optimality of convergence\.

**Instances and GPUs**
+ **Instances**: An AWS [machine learning compute instance](https://aws.amazon.com/sagemaker/pricing/)\. These are also referred to as *nodes*\.
+ **Cluster size**: When using SageMaker's distributed training library, this is the number of instances multiplied by the number of GPUs in each instance\. For example, if you use two ml\.p3\.8xlarge instances in a training job, which have 4 GPUs each, the cluster size is 8\. While increasing cluster size can lead to faster training times, communication between instances must be optimized; Otherwise, communication between the nodes can add overhead and lead to slower training times\. The SageMaker distributed training library is designed to optimize communication between AWS ML compute instances, leading to higher device utilization and faster training times\.

**Distributed Training Solutions**
+ **Data parallelism**: A strategy in distributed training where a training dataset is split up across multiple processing nodes \(such as AWS ML Instances\), and each processing node contains a *replica* of the model\. Each node receives different batches of training data, performs a forward and backward pass, and shares weight updates with the other nodes for synchronization before moving on to the next batch and ultimately another epoch\.
+ **Model parallelism**: A strategy in distributed training where the model partitioned across multiple processing nodes \(such as AWS ML Instances\)\. The model might be complex and have a large number of hidden layers and weights, making it unable to fit in the memory of a single node\. Each node carries a subset of the model, through which the data flows and the transformations are shared and compiled\. The efficiency of model parallelism, in terms of GPU utilization and training time, is heavily dependent on how the model is partitioned and the execution schedule used to perform forward and backward passes\.
+ **Pipeline Execution Schedule** \(**Pipelining**\): The pipeline execution schedule determines the order in which computations \(micro\-batches\) are made and data is processed across devices during model training\. Pipelining is a technique to achieve true parallelization in model parallelism and overcome the performance loss due to sequential computation by having the GPUs compute simultaneously on different data samples\. To learn more, see [Pipeline Execution Schedule](https://docs.aws.amazon.com/sagemaker/latest/dg/model-parallel-core-features.html#model-parallel-pipeline-execution)\. 

**Example**

The following example demonstrates how these terms might be used to describe a training job that uses hybrid model and data parallelism:

A 2\-way data parallelism and 4\-way model parallelism training job is launched with a global batch size of 64 images\. This training job requires a total of 8 GPUs\. Each of the two model replicas processes a per\-replica batch of size 32\. During the forward and backward pass, a per\-replica batch is further divided up into micro\-batches\. These micro\-batches are processed in an interleaved fashion using a pipelined execution schedule\.

The training dataset includes 640 images, and so a single epoch takes 10 iterations\.

## Advanced Concepts<a name="distributed-training-advanced-concepts"></a>

Machine Learning \(ML\) practitioners commonly face two scaling challenges when training models: *scaling model size* and *scaling training data*\. While model size and complexity can result in better accuracy, there is a limit to the model size you can fit into a single CPU or GPU\. Furthermore, scaling model size may result in more computations and longer training times\.

Not all models handle training data scaling equally well\. Some algorithms need to ingest all the training data *in memory* for training\. They only scale vertically, and to bigger and bigger instance types\. Some algorithms have the disadvantage of scaling super\-linearly with dataset size\. For example, the computational load of a nearest\-neighbor search for all records of an N\-rows dataset scales in N squared\. In most cases, scaling training data results in longer training times\.  

Deep Learning \(DL\) is a specific family of ML algorithms consisting of several layers of artificial neural networks\. The most common training method is with mini\-batch Stochastic Gradient Descent \(SGD\)\. In mini\-batch SGD, the model is trained by conducting small iterative changes of its coefficients in the direction that reduces its error\. Those iterations are conducted on equally sized subsamples of the training dataset called *mini\-batches*\. For each mini\-batch, the model is run in each record of the mini\-batch, its error measured and the gradient of the error estimated\. Then the average gradient is measured across all the records of the mini\-batch and provides an update direction for each model coefficient\. One full pass over the training dataset is called an *epoch*\. Model trainings commonly consist of dozens to hundreds of epochs\. Mini\-batch SGD has several benefits: First, its iterative design makes training time theoretically linear of dataset size\. Second, in a given mini\-batch each record is processed individually by the model without need for inter\-record communication other than the final gradient average\. The processing of a mini\-batch is consequently particularly suitable for parallelization and distribution\.  

Parallelizing SGD training by distributing the records of a mini\-batch over different computing devices is called *data parallel distributed training*, and is the most commonly used DL distribution paradigm\. Data parallel training is a relevant distribution strategy to scale the mini\-batch size and process each mini\-batch faster\. However, data parallel training comes with the extra complexity of having to compute the mini\-batch gradient average with gradients coming from all the workers and communicating it to all the workers, a step called *allreduce* that can represent a growing overhead, as the training cluster is scaled, and that can also drastically penalize training time if improperly implemented or implemented over improper hardware subtracts\.  

Data parallel SGD still requires developers to be able to fit at least the model and a single record in a computing devices, like a single CPU or GPU\. When training very large models such as large transformers in Natural Language Processing \(NLP\), or segmentation models over high\-resolution images, there may be situations in which this is not feasible\. An alternative way to break up the workload is to partition the model over multiple computing devices, an approach called *model\-parallel distributed training*\. 

## Strategies<a name="distributed-training-strategies"></a>

Distributed training is usually split by two approaches: data parallel and model parallel\. *Data parallel* is the most common approach to distributed training: You have a lot of data, batch it up, and send blocks of data to multiple CPUs or GPUs \(nodes\) to be processed by the neural network or ML algorithm, then combine the results\. The neural network is the same on each node\. A *model parallel* approach is used with large models that won’t fit in a node’s memory in one piece; it breaks up the model and places different parts on different nodes\. In this situation, you need to send your batches of data out to each node so that the data is processed on all parts of the model\. 

The terms *network* and *model* are often used interchangeably: A large model is really a large network with many layers and parameters\. Training with a large network produces a large model, and loading the model back onto the network with all your pre\-trained parameters and their weights loads a large model into memory\. When you break apart a model to split it across nodes, you’re also breaking apart the underlying network\. A network consists of layers, and to split up the network, you put layers on different compute devices\.

A common pitfall of naively splitting layers across devices is severe GPU under\-utilization\. Training is inherently sequential in both forward and backward passes, and at a given time, only one GPU can actively compute, while the others wait on the activations to be sent\. Modern model parallel libraries solve this problem by using pipeline execution schedules to improve device utilization\. However, only the Amazon SageMaker's distributed model parallel library includes automatic model splitting\. The two core features of the library, automatic model splitting and pipeline execution scheduling, simplifies the process of implementing model parallelism by making automated decisions that lead to efficient device utilization\.

### Train with Data Parallel and Model Parallel<a name="distributed-training-strategies"></a>

If you are training with a large dataset, start with a data parallel approach\. If you run out of memory during training, you may want to switch to a model parallel approach, or try hybrid model and data parallelism\. You can also try the following to improve performance with data parallel:
+ Change your model’s hyperparameters\. 
+ Reduce the batch size\.
+ Keep reducing the batch size until it fits\. If you reduce batch size to 1, and still run out of memory, then you should try model\-parallel training\. 

Try gradient compression \(FP16, INT8\):
+ On NVIDIA TensorCore\-equipped hardware, using [mixed precision training](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html) creates both speed\-up and memory consumption reduction\.
+ SageMaker's distributed data parallelism library supports Automatic Mixed Precision \(AMP\) out of the box\. No extra action is needed to enable AMP other than the framework\-level modifications to your training script\. If gradients are in FP16, the SageMaker data parallelism library runs its `AllReduce` operation in FP16\. For more information about implementing AMP APIs to your training script, see the following resources:
  + [Frameworks \- PyTorch](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html#pytorch) in the *NVIDIA Deep Learning Performace documentation*
  + [Frameworks \- TensorFlow](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html#tensorflow) in the *NVIDIA Deep Learning Performace documentation*
  + [Automatic Mixed Precision for Deep Learning](https://developer.nvidia.com/automatic-mixed-precision) in the *NVIDIA Developer Docs*
  + [Introducing native PyTorch automatic mixed precision for faster training on NVIDIA GPUs](https://pytorch.org/blog/accelerating-training-on-nvidia-gpus-with-pytorch-automatic-mixed-precision/) in the *PyTorch Blog*
  + [TensorFlow mixed precision APIs](https://www.tensorflow.org/guide/mixed_precision) in the *TensorFlow documentation*

Try reducing the input size:
+ Reduce the NLP sequence length if you increase the sequence link, need to adjust the batch size down, or adjust the GPUs up to spread the batch\. 
+ Reduce image resolution\. 

Check if you use batch normalization, since this can impact convergence\. When you use distributed training, your batch is split across GPUs and the effect of a much lower batch size can be a higher error rate thereby disrupting the model from converging\. For example, if you prototyped your network on a single GPU with a batch size of 64, then scaled up to using four p3dn\.24xlarge, you now have 32 GPUs and your per\-GPU batch size drops from 64 to 2\. This will likely break the convergence you saw with a single node\. 

Start with model\-parallel training when: 
+  Your model does not fit on a single device\. 
+ Due to your model size, you’re facing limitations in choosing larger batch sizes, such as if your model weights take up most of your GPU memory and you are forced to choose a smaller, suboptimal batch size\.  

To learn more about the SageMaker distributed libraries, see the following:
+  [SageMaker's Distributed Data Parallel Library](data-parallel.md) 
+  [SageMaker's Distributed Model Parallel](model-parallel.md) 

## Optimize Distributed Training<a name="distributed-training-optimize"></a>

Customize hyperparameters for your use case and your data to get the best scaling efficiency\. In the following discussion, we highlight some of the most impactful training variables and provide references to state\-of\-the\-art implementations so you can learn more about your options\. Also, we recommend that you refer to your preferred framework’s distributed training documentation\. 
+  [Apache MXNet distributed training](https://mxnet.apache.org/versions/1.7/api/faq/distributed_training) 
+  [PyTorch distributed training](https://pytorch.org/tutorials/beginner/dist_overview.html) 
+  [TensorFlow distributed training](https://www.tensorflow.org/guide/distributed_training) 

### Batch Size<a name="batch-size-intro"></a>

SageMaker distributed toolkits generally allow you to train on bigger batches\. For example, if a model fits within a single device but can only be trained with a small batch size, using either model\-parallel training or data parallel training enables you to experiment with larger batch sizes\. 

Be aware that batch size directly influences model accuracy by controlling the amount of noise in the model update at each iteration\. Increasing batch size reduces the amount of noise in the gradient estimation, which can be beneficial when increasing from very small batches sizes, but can result in degraded model accuracy as the batch size increases to large values\.  

**Tip**  
Adjust your hyperparameters to ensure that your model trains to a satisfying convergence as you increase its batch size\.

A number of techniques have been developed to maintain good model convergence when batch is increased\.

### Mini\-Batch Size<a name="distributed-training-optimize"></a>

In SGD, the mini\-batch size quantifies the amount of noise present in the gradient estimation\. A small mini\-batch results in a very noisy mini\-batch gradient, which is not representative of the true gradient over the dataset\. A large mini\-batch results in a mini\-batch gradient close to the true gradient over the dataset and potentially not noisy enough—likely to stay locked in irrelevant minima\. 

To learn more about these techniques, see the following papers:
+ [Accurate, Large Minibatch SGD:Training ImageNet in 1 Hour](https://arxiv.org/pdf/1706.02677.pdf), Goya et al\. 
+ [PowerAI DDL](https://arxiv.org/pdf/1708.02188.pdf), Cho et al\. 
+ [Scale Out for Large Minibatch SGD: Residual Network Training on ImageNet\-1K with Improved Accuracy and Reduced Time to Train](https://arxiv.org/pdf/1711.04291.pdf), Codreanu et al\. 
+ [ImageNet Training in Minutes](https://arxiv.org/pdf/1709.05011.pdf), You et al\. 
+ [Large Batch Training of Convolutional Networks](https://arxiv.org/pdf/1708.03888.pdf), You et al\. 
+ [Large Batch Optimization for Deep Learning: Training BERT in 76 Minutes](https://arxiv.org/pdf/1904.00962.pdf), You et al\. 
+ [Accelerated Large Batch Optimization of BERT Pretraining in 54 minutes](https://arxiv.org/pdf/2006.13484.pdf), Zheng et al\. 
+ [Deep Gradient Compression](https://arxiv.org/abs/1712.01887), Lin et al\. 

## Scenarios<a name="distributed-training-scenarios"></a>

The following sections cover scenarios in which you may want to scale up training, and how you can do so using AWS resources\. 

### Scaling from a Single GPU to Many GPUs<a name="scaling-from-one-GPU"></a>

The amount of data or the size of the model used in machine learning can create situations in which the time to train a model is longer that you are willing to wait\. Sometimes, the training doesn’t work at all because the model or the training data is too large\. One solution is to increase the number of GPUs you use for training\. On an instance with multiple GPUs, like a `p3.16xlarge` that has eight GPUs, the data and processing is split across the eight GPUs\. When you use distributed training libraries, this can result in a near\-linear speedup in the time it takes to train your model\. It takes slightly over 1/8 the time it would have taken on `p3.2xlarge` with one GPU\.


|  |  | 
| --- |--- |
|  Instance type  |  GPUs  | 
|  p3\.2xlarge  |  1  | 
|  p3\.8xlarge  |  4  | 
|  p3\.16xlarge  |  8  | 
|  p3dn\.24xlarge  |  8  | 

**Note**  
The ml instance types used by SageMaker training have the same number of GPUs as the corresponding p3 instance types\. For example, `ml.p3.8xlarge` has the same number of GPUs as `p3.8xlarge` \- 4\. 

### Scaling from a Single Instance to Multiple Instances<a name="scaling-from-one-instance"></a>

If you want to scale your training even further, you can use more instances\. However, you should choose a larger instance type before you add more instances\. Review the previous table to see how many GPUs are in each p3 instance type\. 

If you have made the jump from a single GPU on a `p3.2xlarge` to four GPUs on a `p3.8xlarge`, but decide that you require more processing power, you may see better performance and incur lower costs if you choose a `p3.16xlarge` before trying to increase instance count\. Depending on the libraries you use, when you keep your training on a single instance, performance is better and costs are lower than a scenario where you use multiple instances\.

When you are ready to scale the number of instances, you can do this with SageMaker Python SDK's `estimator` function by setting your `instance_count`\. For example, you can set `instance_type = p3.16xlarge` and   `instance_count = 2`\. Instead of the eight GPUs on a single `p3.16xlarge`, you have 16 GPUs across two identical instances\. The following chart shows [scaling and throughput starting with eight GPUs](https://aws.amazon.com/blogs/machine-learning/scalable-multi-node-training-with-tensorflow/) on a single instance and increasing to 64 instances for a total of 256 GPUs\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/Distributed-Training-in-SageMaker-image.png) 

### Availability Zones and Network Backplane<a name="availability-zones"></a>

With multiple instances, it's important to understand the network that connects the instances, how they read the training data, and how they share information between themselves \(for example, communication between the nodes in the cluster when doing an `AllReduce` operation\)\. 

First, your instances need to be in the same Region and same Availability Zone\. For example, instances in us\-west\-2 must all be in us\-west\-2a\. When you use the SageMaker Python SDK, this is handled for you\. If you use Amazon EC2 and orchestrate your own training clusters, you need to be aware of this, or your training speeds suffer\. 

Your training data should also be in the same Availability Zone\. When you use a SageMaker estimator, you pass in the Region and the S3 bucket, and if the data is not in the Region you set, you get an error\.  

### Optimized GPU, Network, and Storage<a name="optimized-GPU"></a>

The `p3dn.24xlarge` instance type was designed for fast local storage and a fast network backplane with up to 100 gigabits, and we highly recommend it as the most performant option for distributed training\. SageMaker supports streaming data modes from S3, referred to as `pipe` mode\. For HPC loads like distributed training, we recommend [Amazon Fsx](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html) for your file storage\. 

### Custom Training Scripts<a name="custom-training-scripts"></a>

While SageMaker makes it simple to deploy and scale the number of instances and GPUs, depending on your framework of choice, managing the data and results can be very challenging, which is why external supporting libraries are often used\. This most basic form of distributed training requires modification of your training script to manage the data distribution\. 

SageMaker also supports Horovod and implementations of distributed training native to each major deep learning framework\. If you choose to use examples from these frameworks, you can follow SageMaker’s [container guide](https://docs.aws.amazon.com/sagemaker/latest/dg/docker-containers.html) for Deep Learning Containers, and various [example notebooks](https://sagemaker-examples.readthedocs.io/en/latest/training/bring_your_own_container.html) that demonstrate implementations\. 

## SageMaker Built\-In Distributed Training Features<a name="distributed-training-built-in"></a>

The [SageMaker built\-in libraries of algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html) consists of 18 popular machine learning algorithms\. Many of them were rewritten from scratch to be scalable and distributed out of the box\. If you want to use distributed **deep learning** training code, we recommend Amazon SageMaker’s distributed training libraries\. SageMaker’s distributed training libraries make it easier for you to write highly scalable and cost\-effective custom data parallel and model parallel deep learning training jobs\. 

SageMaker distributed training libraries offer both data parallel and model parallel training strategies\. It combines software and hardware technologies to improve inter\-GPU and inter\-node communications\. It extends SageMaker’s training capabilities with built\-in options that require only small code changes to your training scripts\.  
+  [SageMaker's Distributed Data Parallel Library](data-parallel.md) 
+  [SageMaker's Distributed Model Parallel](model-parallel.md) 