# Core Features of SageMaker Distributed Model Parallel<a name="model-parallel-core-features"></a>

Amazon SageMaker's distributed model parallel makes model parallelism more accessible by providing automated model splitting and sophisticated pipeline execution scheduling\. The model splitting algorithms can optimize for speed or memory consumption\. The library also supports manual partitioning\. When you use the library, training is executed in a pipelined fashion over microbatches to maximize GPU usage\.

You can configure these features using a few lines of code when you create your training script and define your SageMaker PyTorch or Tensorflow `Estimator`\. Use the following sections to learn more about these core features of the library\.

## Automated Model Splitting<a name="model-parallel-automated-model-splitting"></a>

When you use SageMaker's model parallel library, you can take advantage of *automated model splitting*, also referred to as *automated model partitioning*\. The library uses a partitioning algorithm that balances memory, minimizes communication between devices, and optimizes performance\. You can configure the automated partitioning algorithm to optimize for speed or memory\. 

Alternatively, you can use manual model splitting\. We recommend automated model splitting, unless you are very familiar with the model architecture and have a good idea of how to efficiently partition your model\.

### How It Works<a name="model-parallel-automated-model-splitting-how-it-works"></a>

Auto\-partitioning occurs during the first training step, when the `smp.step`\-decorated function is first called\. During this call, the library first constructs a version of the model on the CPU RAM \(to avoid GPU memory limitations\), and then analyzes the model graph and makes a partitioning decision\. Based on this decision, each model partition is loaded on a GPU, and only then the first step is executed\. Because of these analysis and partitioning steps, the first training step might take longer\. 

In either framework, the library manages the communication between devices through its own backend, which is optimized for AWS infrastructure\.

The auto\-partition design adapts to the characteristics of the framework, and the library does the partitioning at the granularity level that is more natural in each framework\. For instance, in TensorFlow, each specific operation can be assigned to a different device, whereas in PyTorch, the assignment is done at the module level, where each module consists of multiple operations\. The follow section reviews the specifics of the design in each framework\.

#### Automated Model Splitting with TensorFlow<a name="model-parallel-auto-model-split-tf"></a>

The model parallel library analyzes the sizes of the trainable variables and the graph structure, and internally uses a graph partitioning algorithm\. This algorithm comes up with a device assignment for each operation, with the objective of minimizing the amount of communication needed across devices, subject to two constraints: 
+ Balancing the number of variables stored in each device
+ Balancing the number of operations executed in each device

If you specify `speed` for `optimize` \(in the model parallel parameters in the Python SDK\), the library tries to balance the number of operations and `tf.Variable` objects in each device\. Otherwise, it tries to balance the total size of `tf.Variables`\.

Once the partitioning decision is made, the library creates a serialized representation of the subgraph that each device needs to execute and imports them onto each device\. While partitioning, the library places operations that consume the same `tf.Variable` and operations that are part of the same Keras layer onto the same device\. It also respects the colocation constraints imposed by TensorFlow\. This means that, for example, if there are two Keras layers that share a `tf.Variable`, then all operations that are part of these layers are placed on a single device\.

#### Automated Model Splitting with PyTorch<a name="model-parallel-auto-model-split-pt"></a>

During the first training step, the model parallel library internally runs a tracing step that is meant to construct the model graph and determine the tensor and parameter shapes\. After this tracing step, the library constructs a tree, which consists of the nested `nn.Module` objects in the model, as well as additional data gathered from tracing, such as the amount of stored `nn.Parameters`, and execution time for each `nn.Module`\. 

Next, the library traverses this tree from the root and runs a partitioning algorithm that assigns each `nn.Module` to a device, which balances computational load \(measured by module execution time\) and memory use \(measured by the total stored `nn.Parameter` size and activations\)\. If multiple `nn.Modules` share the same `nn.Parameter`, then these modules are placed on the same device to avoid maintaining multiple versions of the same parameter\. Once the partitioning decision is made, the assigned modules and weights are loaded to their devices\.

#### Comparison of Automated Model Splitting Between Frameworks<a name="model-parallel-auto-model-split-comparison"></a>

In TensorFlow, the fundamental unit of computation is a `tf.Operation`, and TensorFlow represents the model as a directed acyclic graph \(DAG\) of `tf.Operation`s, and therefore model parallel library partitions this DAG so that each node goes to one device\. Crucially, `tf.Operation` objects are sufficiently rich with customizable attributes, and they are universal in the sense that every model is guaranteed to consist of a graph of such objects\. 

PyTorch on the other hand, does not have an equivalent notion of operation that is sufficiently rich and universal\. The closest unit of computation in PyTorch that has these characteristics is an `nn.Module`, which is at a much higher granularity level, and this is why the library does partitioning at this level in PyTorch\.

## Manual Model Splitting<a name="model-parallel-manual-model-splitting"></a>

If you want to manually specify how your model is partitioned across devices, you can use manual model splitting by using `smp.partition` context managers\. 

To use this option, set `auto_partition` to `False`, and define a `default_partition` in the SageMaker Python SDK\. Any operation that is not explicitly placed on a partition through the `smp.partition` context manager is executed on the `default_partition`\. In this case, the automated splitting logic is bypassed, and each operation is placed based on your specification\. Based on the resulting graph structure, the model parallel library creates a pipelined execution schedule automatically\.

## Pipeline Execution Schedule<a name="model-parallel-pipeline-execution"></a>

A core feature of SageMaker's distributed model parallel library is *pipelined execution*, which determines the order in which computations are made and data is processed across devices during model training\. Pipelining is a technique to achieve true parallelization in model parallelism, by having the GPUs compute simultaneously on different data samples, and to overcome the performance loss due to sequential computation\.

Pipelining is based on splitting a mini\-batch into microbatches, which are fed into the training pipeline one\-by\-one and follow an execution schedule defined by the library runtime\. A *microbatch* is a smaller subset of a given training mini\-batch\. The pipeline schedule determines which microbatch is executed by which device for every time slot\. 

For example, depending on the pipeline schedule and the model partition, GPU `i` might perform \(forward or backward\) computation on microbatch `b` while GPU `i+1` performs computation on microbatch `b+1`, thereby keeping both GPUs active at the same time\. During a single forward or backward pass, execution flow for a single microbatch might visit the same device multiple times, depending on the partitioning decision\. For instance, an operation that is at the beginning of the model might be placed on the same device as an operation at the end of the model, while the operations in between are on different devices, which means this device is visited twice\.

The library offers two different pipeline schedules, *simple* and *interleaved*, which can be configured using the `pipeline` parameter in the SageMaker Python SDK\. In most cases, interleaved pipeline can achieve better performance by utilizing the GPUs more efficiently\.

### Interleaved Pipeline<a name="model-parallel-pipeline-execution-interleaved"></a>

In an interleaved pipeline, backward execution of the microbatches is prioritized whenever possible\. This allows quicker release of the memory used for activations, using memory more efficiently\. It also allows for scaling the number of microbatches higher, reducing the idle time of the GPUs\. At steady\-state, each device alternates between running forward and backward passes\. This means that the backward pass of one microbatch may run before the forward pass of another microbatch finishes\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/model-parallel/interleaved-pipeline-execution.png)

The preceding figure illustrates an example execution schedule for the interleaved pipeline over 2 GPUs\. In the figure, F0 represents the forward pass for microbatch 0, and B1 represents the backward pass for microbatch 1\. **Update** represents the optimizer update of the parameters\. GPU0 always prioritizes backward passes whenever possible \(for instance, executes B0 before F2\), which allows for clearing of the memory used for activations earlier\.

### Simple Pipeline<a name="model-parallel-pipeline-execution-simple"></a>

A simple pipeline, by contrast, finishes running the forward pass for each microbatch before starting the backward pass\. This means that it only pipelines the forward pass and backward pass stages within themselves\. The following figure illustrates an example of how this works, over 2 GPUs\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/model-parallel/simple-pipeline-execution.png)

### Pipelining Execution in Specific Frameworks<a name="model-parallel-pipeline-frameworks"></a>

Use the following sections to learn about the framework\-specific pipeline scheduling decisions SageMaker's distributed model parallel library makes for Tensorflow and PyTorch\. 

#### Pipeline Execution with TensorFlow<a name="model-parallel-pipeline-execution-interleaved-tf"></a>

The following image is an example of a TensorFlow graph partitioned by the model parallel library, using automated model splitting\. When a graph is split, each resulting subgraph is replicated B times \(except for the variables\), where B is the number of microbatches\. In this figure, each subgraph is replicated 2 times \(B=2\)\. An `SMPInput` operation is inserted at each input of a subgraph, and an `SMPOutput` operation is inserted at each output\. These operations communicate with the library backend to transfer tensors to and from each other\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/model-parallel/interleaved-pipeline-tf.png)

The following image is an example of 2 subgraphs split with B=2 with gradient operations added\. The gradient of a `SMPInput` op is a `SMPOutput` op, and vice versa\. This enables the gradients to flow backwards during back\-propagation\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/model-parallel/interleaved-pipeline-tf.gif)

This GIF demonstrates an example interleaved pipeline execution schedule with B=2 microbatches and 2 subgraphs\. Each device sequentially executes one of the subgraph replicas to improve GPU utilization\. As B grows larger, the fraction of idle time slots goes to zero\. Whenever it is time to do \(forward or backward\) computation on a specific subgraph replica, the pipeline layer signals to the corresponding blue `SMPInput` operations to start executing\.

Once the gradients from all microbatches in a single mini\-batch are computed, the library combines the gradients across microbatches, which can then be applied to the parameters\. 

#### Pipeline Execution with PyTorch<a name="model-parallel-pipeline-execution-interleaved-pt"></a>

Conceptually, pipelining follows a similar idea in PyTorch\. However, since PyTorch does not involve static graphs and so the model parallel library's PyTorch feature uses a more dynamic pipelining paradigm\. 

As in TensorFlow, each batch is split into a number of microbatches, which are executed one at a time on each device\. However, the execution schedule is handled via execution servers launched on each device\. Whenever the output of a submodule that is placed on another device is needed on the current device, an execution request is sent to the execution server of the remote device along with the input tensors to the submodule\. The server then executes this module with the given inputs and returns the response to the current device\.

Since the current device is idle during the remote submodule execution, the local execution for the current microbatch pauses, and the library runtime switches execution to another microbatch which the current device can actively work on\. The prioritization of microbatches is determined by the chosen pipeline schedule\. For an interleaved pipeline schedule, microbatches that are in the backward stage of the computation are prioritized whenever possible\.