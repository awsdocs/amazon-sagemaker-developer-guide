# Distributed Training<a name="distributed-training"></a>

**Topics**
+ [Get Started with Distributed Training](#distributed-training-get-started)
+ [Basic Distributed Training Concepts](#distributed-training-basic-concepts)
+ [Advanced Concepts](#distributed-training-advanced-concepts)
+ [Strategies](#distributed-training-strategies)
+ [Optimize Distributed Training](#distributed-training-optimize)
+ [Scenarios](#distributed-training-scenarios)
+ [SageMaker Built\-In Distributed Training Features](#distributed-training-built-in)
+ [SageMaker distributed data parallel](data-parallel.md)
+ [SageMaker distributed model parallel](model-parallel.md)
+ [Distributed Training Jupyter Notebok Examples](distributed-training-notebook-examples.md)
+ [Troubleshooting](distributed-troubleshooting.md)

## Get Started with Distributed Training<a name="distributed-training-get-started"></a>

If you’re familiar with distributed training, follow one of the links to your preferred strategy or framework to get started\. Otherwise, continue on to the next section to learn some distributed training concepts\. 

 **SageMaker distributed training libraries:** 
+  [SageMaker distributed data parallel](data-parallel.md) 
+  [SageMaker distributed model parallel](model-parallel.md) 

## Basic Distributed Training Concepts<a name="distributed-training-basic-concepts"></a>

 SageMaker’s distributed training libraries use the following distributed training terms and features\. 

**Batch size**: The number of records from the dataset selected for each interval to send to the GPUs in the cluster\. In an image example, if you send out 32 images per GPU, your batch size is 32\. Advanced implementations provide some guidance on optimizing batch size in a warm\-up period and throughout training\. These suggestions vary depending on the data you are using\. For example, how you optimize for image data will differ from how you handle language data\. 

**Cluster size**: The number of instances multiplied by the number of GPUs in each instance\. This is the amount of raw processing power you use to train a model in a single training job\. The number of GPUs you bring to bear on a training job has the most impact on how long it takes to train a model\. However, just throwing processing power at the problem is not enough\. Communication between the instances \(usually referred to as nodes in a cluster\) can create so much overhead that the training time is worse than if you had fewer nodes\. This is where SageMaker distributed training and other advanced distributed training frameworks step in with optimized communication solutions\. 

**Data parallel**: A strategy in distributed training where the dataset is split up across multiple processing nodes\. Each node sees different batches from the dataset, makes its calculations, and shares them back to the other nodes for synchronization before moving on to the next batch and ultimately another epoch\. 

**Dataset**: The entire corpus of data on which you’re training\. The dataset is often split into subsets: one for training and one for validation\. For example, you might split the dataset and use 80% of it for training \(the training set\), then use the remaining 20% to test the current accuracy of the model \(the validation set\) while it is still training\. 

**Device batch**: The number of records seen by a device during an iteration\. This is a fraction of the mini\-batch\.

**Epoch**: One cycle through the entire dataset\. Multiple intervals complete a batch, and multiple batches eventually complete an epoch\. Multiple epochs are run until the accuracy of the model reaches an acceptable level—or, looked at another way, when the error rate drops below an acceptable level\. 

**Interval or iteration**: A distribution of a batch of data\. A large amount of data must be broken up into batches and sent to the GPUs in mini\-batches\. The number of intervals is equal to the number of batches it takes to get through the entire dataset\. 

**Learning rate**: The amount that values should be changed between epochs\. As the model is refined, its internal weights are nudged and error rates are checked to see if the model improves\. A typical learning rate is 0\.1 or 0\.01; 0\.01 is a much smaller adjustment and could cause the training to take a long time to converge, whereas 0\.1 is much larger and can cause the training to overshoot\. It is one of the primary hyperparameters that you can adjust for training your model\. You may also want to try a warm\-up period in which you set an initial low learning rate, so that the neural net has a chance to stabilize newly initialized layers, and with each subsequent epoch you gradually increase the learning rate\. 

**Mini\-batch size**: The number of records over which the model error gradient is estimated at each iteration\. It is the number of GPUs times the batch size\. For example, if batch size is 32 and you have 256 GPUs, your mini\-batch size is 8,192\. Changing the mini\-batch size will generally change the performance of your model and the required time\-to\-results for a given performance goal\. 

**Microbatch**: A smaller subset of a given training mini\-batch\. Model parallel pipelining is based on splitting a mini\-batch into microbatches, which are fed into the training pipeline one\-by\-one and follow an execution schedule defined by SMP runtime\.

**Model parallel**: A strategy in distributed training in which the model is split up across multiple processing nodes\. The model may be very complex and have a huge number of layers and weights inside it, making it unable to fit in the memory of a single node\. Each node carries a subset of the model, through which the data flows and the transformations are shared and compiled\. 

**Pipeline Execution Schedule** \(**Pipelining**\): This defines pipelined execution when training a model with model parallelism\. The pipeline execution schedule determines the order in which computations are made and data is processed across devices during model training\. Pipelining is a technique to achieve true parallelization in model parallelism and overcome the performance loss due to sequential computation by having the GPUs compute simultaneously on different data samples\. 

## Advanced Concepts<a name="distributed-training-advanced-concepts"></a>

Machine Learning \(ML\) practitioners commonly face two scaling challenges when training models: *scaling model size* and *scaling training data*\. While model size and complexity can result in better accuracy, there is a limit to the model size you can fit into a single CPU or GPU\. Furthermore, scaling model size may result in more computations and longer training times\. 

Not all models handle training data scaling equally well\. Some algorithms need to ingest all the training data *in memory* for training\. They only scale vertically, and to bigger and bigger instance types\. Some algorithms have the disadvantage of scaling super\-linearly with dataset size\. For example, the computational load of a nearest\-neighbor search for all records of an N\-rows dataset scales in N squared\. In most cases, scaling training data results in longer training times\.  

Deep Learning \(DL\) is a specific family of ML algorithms consisting of several layers of artificial neural networks\. The most common training method is with mini\-batch Stochastic Gradient Descent \(SGD\)\. In mini\-batch SGD, the model is trained by conducting small iterative changes of its coefficients in the direction that reduces its error\. Those iterations are conducted on equally sized subsamples of the training dataset called *mini\-batches*\. For each mini\-batch, the model is run in each record of the mini\-batch, its error measured and the gradient of the error estimated\. Then the average gradient is measured across all the records of the mini\-batch and provides an update direction for each model coefficient\. One full pass over the training dataset is called an *epoch*\. Model trainings commonly consist of dozens to hundreds of epochs\. Mini\-batch SGD has several benefits: First, its iterative design makes training time theoretically linear of dataset size\. Second, in a given mini\-batch each record is processed individually by the model without need for inter\-record communication other than the final gradient average\. The processing of a mini\-batch is consequently particularly suitable for parallelization and distribution\.  

Parallelizing SGD training by distributing the records of a mini\-batch over different computing devices is called *data\-parallel distributed training*, and is the most commonly used DL distribution paradigm\. Data\-parallel training is a relevant distribution strategy to scale the mini\-batch size and process each mini\-batch faster\. However, data\-parallel training comes with the extra complexity of having to compute the mini\-batch gradient average with gradients coming from all the workers and communicating it to all the workers, a step called *allreduce* that can represent a growing overhead, as the training cluster is scaled, and that can also drastically penalize training time if improperly implemented or implemented over improper hardware substracts\.  

Data\-parallel SGD still requires developers to be able to fit at least the model and a single record in a computing devices, like a single CPU or GPU\. When training very large models such as large transformers in Natural Language Processing \(NLP\), or segmentation models over high\-resolution images, there may be situations in which this is not feasible\. An alternative way to break up the workload is to partition the model over multiple computing devices, an approach called > *model\-parallel distributed training*\. 

## Strategies<a name="distributed-training-strategies"></a>

Distributed training is usually split by two approaches: data parallel and model parallel\. *Data parallel* is the most common approach to distributed training: You have a lot of data, batch it up, and send blocks of data to multiple CPUs or GPUs \(nodes\) to be processed by the neural network or ML algorithm, then combine the results\. The neural network is the same on each node\. A *model parallel* approach is used with large models that won’t fit in a node’s memory in one piece; it breaks up the model and places different parts on different nodes\. In this situation, you need to send your batches of data out to each node so that the data is processed on all parts of the model\. 

The terms *network* and *model* are often used interchangeably: A large model is really a large network with many layers and parameters\. Training with a large network produces a large model, and loading the model back onto the network with all your pre\-trained parameters and their weights loads a large model into memory\. When you break apart a model to split it across nodes, you’re also breaking apart the underlying network\. A network consists of layers, and to split up the network, you put layers on different compute devices\.

A common pitfall of naively splitting layers across devices is severe GPU under\-utilization\. Training is inherently sequential in both forward and backward passes, and at a given time, only one GPU can actively compute, while the others wait on the activations to be sent\. Modern model parallel libraries solve this problem by using pipeline execution schedules to improve device utilization\. However, only SageMaker distributed model parallel library \(SMP\) includes automatic model splitting\. The two core features of SMP, automatic model splitting and pipeline execution scheduling, simplifies the process of implementing model parallelism by making automated decisions that lead to efficient device utilization\.

### Which Is “Better”: Data Parallel or Model Parallel?<a name="data-parallel-vs-model-parallel"></a>

Despite their names, data\-parallel training is truly parallel computing, whereas model\-parallel training is more akin to serialized computing\. If you can use data\-parallel training, you should, as training is faster\. However, this isn’t always possible, and so for large models, you must use model\-parallel training\. There are parts of a model\-parallel flow that can be parallelized, but some parts of the network bottleneck the others and require you to wait forthe network to finish processing data in a serial fashion\. However, SageMaker’s model parallelism library uses sophisticated pipelining, enabling the library to parallelize work by having devices compute on different microbatches at the same time\. This approaches true parallelism, even if the underlying model consists of sequential, non\-parallelizable computational steps\. 

### Choosing Between Data Parallel and Model Parallel<a name="distributed-training-strategies"></a>

Start with a data parallel approach\. If you run out of memory during training, you may want to switch to a model parallel approach\. However, consider these alternatives before trying model\-parallel training: 
+ Change your model’s hyperparameters\. 
+ Reduce the batch size\.
+ Keep reducing the batch size until it fits\. If you reduce batch size to 1, and still run out of memory, then you should try model\-parallel training\. 

 Try gradient compression \(fp16, fp8\):
+ On NVIDIA TensorCore\-equipped hardware, using [mixed\-precision training](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html) creates both speed\-up and memory consumption reduction\.

Try reducing the input size:
+ Reduce the NLP sequence length if you increase the sequence link, need to adjust the batch size down, or adjust the GPUs up to spread the batch\. 
+ Reduce image resolution\. 

Check if you use batch normalization, since this can impact convergence\. 64 versus 2 can break things\. 

Start with model\-parallel training when: 
+  Your model does not fit on a single device\. 
+ Due to your model size, you’re facing limitations in choosing larger batch sizes, such as if your model weights take up most of your GPU memory and you are forced to choose a smaller, suboptimal batch size\.  

To learn more about the SageMaker distributed libraries, see the following:
+  [SageMaker distributed data parallel](data-parallel.md) 
+  [SageMaker distributed model parallel](model-parallel.md) 

## Optimize Distributed Training<a name="distributed-training-optimize"></a>

Customize hyperparameters for your use case and your data to get the best scaling efficiency\. In the following discussion, we highlight some of the most impactful training variables and provide references to state\-of\-the\-art implementations so you can learn more about your options\. Also, we recommend that you refer to your preferred framework’s distributed training documentation\. 
+  [Apache MXNet distributed training](https://mxnet.apache.org/versions/1.7/api/faq/distributed_training) 
+  [PyTorch distributed training](https://pytorch.org/tutorials/beginner/dist_overview.html) 
+  [TensorFlow distributed training](https://www.tensorflow.org/guide/distributed_training) 

### Batch Size<a name="batch-size-intro"></a>

SageMaker distributed toolkits generally allow you to train on bigger batches\. For example, if a model fits within a single device but can only be trained with a small batch size, using either model\-parallel training or data\-parallel training enables you to experiment with larger batch sizes\. 

Be aware that batch size directly influences model accuracy by controlling the amount of noise in the model update at each iteration\. Increasing batch size reduces the amount of noise in the gradient estimation, which can be beneficial when increasing from very small batches sizes, but can result in degraded model accuracy as the batch size increases to large values\.  

**Tip**  
Adjust your hyperparameters to ensure that your model trains to a satisfying convergence as you increase its batch size\.

A number of techniques have been developed to maintain good model convergence when batch is increased\.

To learn more about these techniques, see the following papers:
+ \[1\] [Accurate, Large Minibatch SGD:Training ImageNet in 1 Hour](https://arxiv.org/pdf/1706.02677.pdf), Goyal et al\. 
+ \[2\] [PowerAI DDL](https://arxiv.org/pdf/1708.02188.pdf), Cho et al\. 
+ \[3\] [Scale Out for Large Minibatch SGD: Residual Network Training on ImageNet\-1K with Improved Accuracy and Reduced Time to Train](https://arxiv.org/pdf/1711.04291.pdf), Codreanu et al\. 
+ \[4\] [ImageNet Training in Minutes](https://arxiv.org/pdf/1709.05011.pdf), You et al\. 
+ \[5\] [Large Batch Training of Convolutional Networks](https://arxiv.org/pdf/1708.03888.pdf), You et al\. 
+ \[6\] [Large Batch Optimization for Deep Learning: Training BERT in 76 Minutes](https://arxiv.org/pdf/1904.00962.pdf), You et al\. 
+ \[7\] [Accelerated Large Batch Optimization of BERT Pretraining in 54 minutes](https://arxiv.org/pdf/2006.13484.pdf), Zheng et al\. 
+ \[8\] [Deep Gradient Compression](https://arxiv.org/abs/1712.01887), Lin et al\. 

### Mini\-Batch Size<a name="distributed-training-optimize"></a>

In SGD, the mini\-batch size quantifies the amount of noise present in the gradient estimation\. A small mini\-batch results in a very noisy mini\-batch gradient, which is not representative of the true gradient over the dataset\. A large mini\-batch results in a mini\-batch gradient close to the true gradient over the dataset and potentially not noisy enough—likely to stay locked in irrelevant minima\. 

## Scenarios<a name="distributed-training-scenarios"></a>

The following sections cover scenarios in which you may want to scale up training, and how you can do so using AWS resources\. 

### Scaling from a Single GPU to Many GPUs<a name="scaling-from-one-GPU"></a>

The amount of data or the size of the model used in machine learning can create situations in which the time to train a model is longer that you are willing to wait\. Sometimes, the training doesn’t work at all because the model or the training data is too large\. One solution is to increase the number of GPUs you use for training\. On an instance with multiple GPUs, like a `p3.16xlarge` that has eight GPUs, the data and processing is split across the eight GPUs, and producing a near\-linear speedup in the time it takes to train your model\. It takes slightly over 1/8 the time it would have taken on `p3.2xlarge` with one GPU\.  


|  |  | 
| --- |--- |
|  Instance type  |  GPUs  | 
|  p3\.2xlarge  |  1  | 
|  p3\.8xlarge  |  4  | 
|  p3\.16xlarge  |  8  | 
|  p3dn\.24xlarge  |  8  | 

### Scaling from a Single Instance to Multiple Instances<a name="scaling-from-one-instance"></a>

If you want to scale your training even further, you can use more instances\. However, you should choose a larger instance type before you add more instances\. Review the previous table to see how many GPUs are in each p3 instance type\. 

If you have made the jump from a single GPU on a `p3.2xlarge` to four GPUs on a `p3.8xlarge`, but decide that you require more processing power, you should always choose a `p3.16xlarge` before trying to increase instance count\. When you keep your training on a single instance, performance is better than when you use multiple instances\. Also, your costs are lower\. 

When you are ready to scale the number of instances, you can do this with SageMaker PythonSDK’s `estimator` function by setting your `instance_type = p3.16xlarge` and   `instance_count = 2`\. Instead of the eight GPUs on a single `p3.16xlarge`, you have 16 GPUs across two identical instances\. The following chart shows [scaling and throughput starting with eight GPUs](https://aws.amazon.com/blogs/machine-learning/scalable-multi-node-training-with-tensorflow/) on a single instance and increasing to 64 instances for a total of 256 GPUs\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/Distributed-Training-in-SageMaker-image.png) 

### Availability Zones and Network Backplane<a name="availability-zones"></a>

With multiple instances, it's important to understand the network that connects the instances, how they read the training data, and how they share information between themselves \(for example, communication between the nodes in the cluster when doing an `AllReduce` operation\)\. 

First, your instances need to be in the same Region and same Availability Zone\. For example, instances in us\-west\-2 must all be in us\-west\-2a\. When you use the SageMaker Python SDK, this is handled for you\. If you use Amazon EC2 and orchestrate your own training clusters, you need to be aware of this, or your training speeds suffer\.  

Your training data should also be in the same availability zone\. When you use a SageMaker estimator, you pass in the Region and the S3 bucket, and if the data is not in the Region you set, you get an error\.  

### Optimized GPU, Network, and Storage<a name="optimized-GPU"></a>

The `p3dn.24xlarge` instance type was designed for fast local storage and a fast network backplane with up to 100 gigabits, and we highly recommend it as the most performant option for distributed training\. SageMaker supports streaming data modes from S3, referred to as `pipe` mode\. For HPC loads like distributed training, we recommend [Amazon Fsx](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html) for your file storage\. 

### Custom Training Scripts<a name="custom-training-scripts"></a>

While SageMaker makes it simple to deploy and scale the number of instances and GPUs, depending on your framework of choice, managing the data and results can be very challenging, which is why external supporting libraries are often used\. This most basic form of distributed training requires modification of your training script to manage the data distribution\. 

SageMaker also supports Horovod and implementations of distributed training native to each major deep learning framework\. If you choose to use examples from these frameworks, you can follow SageMaker’s container guide for Deep Learning Containers, and various example notebooks that demonstrate implementations\. 

## SageMaker Built\-In Distributed Training Features<a name="distributed-training-built-in"></a>

The [SageMaker built\-in libraries of algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html) consists of 18 popular machine learning algorithms\. Many of them were rewritten from scratch to be scalable and distributed out of the box\. If you want to use distributed **deep learning** training code, we recommend Amazon SageMaker’s distributed training libraries\. SageMaker’s distributed training libraries make it easier for you to write highly scalable and cost\-effective custom data parallel and model parallel deep learning training jobs\. 

SageMaker distributed training libraries offer both data\-parallel and model\-parallel training strategies\. It combines software and hardware technologies to improve inter\-GPU and inter\-node communications\. It extends SageMaker’s training capabilities with built\-in options that require only small code changes to your training scripts\.  
+  [SageMaker distributed data parallel](data-parallel.md) 
+  [SageMaker distributed model parallel](model-parallel.md) 