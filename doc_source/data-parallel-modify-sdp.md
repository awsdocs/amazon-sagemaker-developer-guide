# Modify Your Training Script using the SageMaker Data Parallel Library<a name="data-parallel-modify-sdp"></a>

## Script Modification Overview<a name="data-parallel-modify-sdp-overview"></a>

SageMaker's distributed data parallel library \(the library\) APIs are designed for ease of use, and to provide seamless integration with existing distributed training toolkits\.
+ *SageMaker Python SDK with the library API*: In most cases, all you have to change in your training script is the Horovod or other data parallel library import statements\. Swap these out with the the SageMaker data parallel library equivalents\.
+ *Focus on your model training without infrastructure management*: When training a deep learning model with the library on SageMaker, you can focus on your model training, while SageMaker does cluster management: brings up the nodes and creates the cluster, completes the training, then tears down the cluster\. 

 To customize your own training script, you need to do the following: 
+ You must provide TensorFlow/PyTorch training scripts that are adapted to use the library\. The following sections provide example code for this\. 
+ Your input data must be in an S3 bucket or in [FSx](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html) in the AWS Region that you will use to launch your training job\. If you use the Jupyter Notebooks provided, create a SageMaker notebook instance in the same Region as the bucket that contains your input data\. For more information about storing your training data, refer to the [SageMaker Python SDK data inputs](https://sagemaker.readthedocs.io/en/stable/overview.html#use-file-systems-as-training-input) documenation\. 

**Tip**  
Consider using FSx instead of Amazon S3 to increase training performance\. It has higher throughput and lower latency than Amazon S3\. 

Use the following sections to see examples of training scripts that can be used to convert your TensorFlow or PyTorch training scripts\. Then, use one of the example notebooks for your template to launch a training job\. You’ll need to swap your training script with the one that came with the notebook and modify any input functions as necessary\. Once you have launched a training job, you can monitor it using Amazon CloudWatch\. 

Then you can see how to deploy your trained model to an endpoint by following one of the example notebooks for deploying a model\. 

Finally, you can follow an example notebook to test inference on your deployed model\. 

## Modify a TensorFlow 2\.x Training Script Using SMD Data Parallel<a name="data-parallel-modify-sdp-tf2"></a>

 The following steps show you how to convert a TensorFlow 2\.3\.1 or 2\.4\.1 training script to utilize SageMaker's distributed data parallel library\.  

The library APIs are designed to be similar to Horovod APIs\. Refer to the [SageMaker distributed data parallel TensorFlow API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html#api-documentation) for additional details on each API that the library offers for TensorFlow\. 

1. Import the library's TensorFlow client and initialize it: 

   ```
   import smdistributed.dataparallel.tensorflow as sdp 
   sdp.init()
   ```

1. Pin each GPU to a single `smdistributed.dataparallel` process with `local_rank`—this refers to the relative rank of the process within a given node\. The `sdp.tensorflow.local_rank()` API provides you the local rank of the device\. The leader node is rank 0, and the worker nodes are rank 1, 2, 3, and so on\. This is invoked in the next code block as `sdp.local_rank()`\. `set_memory_growth` is not directly related to SageMaker distributed, but must be set for distributed training with TensorFlow\. 

   ```
   gpus = tf.config.experimental.list_physical_devices('GPU')
   for gpu in gpus:
       tf.config.experimental.set_memory_growth(gpu, True)
   if gpus:
       tf.config.experimental.set_visible_devices(gpus[sdp.local_rank()], 'GPU')
   ```

1. Scale the learning rate by the number of workers\. The `sdp.tensorflow.size()` API provides you the number of workers in the cluster\. This is invoked in the next code block as `sdp.size()`\. 

   ```
   learning_rate = learning_rate * sdp.size()
   ```

1. Use the library’s `DistributedGradientTape`  to optimize `AllReduce` operations during training\. This wraps `tf.GradientTape`\.  

   ```
   with tf.GradientTape() as tape:
         output = model(input)
         loss_value = loss(label, output)
       
   # SageMaker data parallel: Wrap tf.GradientTape with the library's DistributedGradientTape
   tape = sdp.DistributedGradientTape(tape)
   ```

1. Broadcast the initial model variables from the leader node \(rank 0\) to all the worker nodes \(ranks 1 through n\)\. This is needed to ensure a consistent initialization across all the worker ranks\. For this, you use the `sdp.tensorflow.broadcast_variables` API after the model and optimizer variables are initialized\. This is invoked in the next code block as `sdp.broadcast_variables()`\. 

   ```
   sdp.broadcast_variables(model.variables, root_rank=0)
   sdp.broadcast_variables(opt.variables(), root_rank=0)
   ```

1. Finally, modify your script to save checkpoints only on the leader node\. The leader nodehas a synchronized model\. This also avoids worker nodes overwriting the checkpoints and possibly corrupting the checkpoints\. 

   ```
   if sdp.rank() == 0:
       checkpoint.save(checkpoint_dir)
   ```

The following is an example TensorFlow2 training script for distributed training with the library: 

```
import tensorflow as tf

# SageMaker data parallel: Import the library TF API
import smdistributed.dataparallel.tensorflow as sdp

# SageMaker data parallel: Initialize the library
sdp.init()

gpus = tf.config.experimental.list_physical_devices('GPU')
for gpu in gpus:
    tf.config.experimental.set_memory_growth(gpu, True)
if gpus:
    # SageMaker data parallel: Pin GPUs to a single library process
    tf.config.experimental.set_visible_devices(gpus[sdp.local_rank()], 'GPU')

# Prepare Dataset
dataset = tf.data.Dataset.from_tensor_slices(...)

# Define Model
mnist_model = tf.keras.Sequential(...)
loss = tf.losses.SparseCategoricalCrossentropy()

# SageMaker data parallel: Scale Learning Rate
# LR for 8 node run : 0.000125
# LR for single node run : 0.001
opt = tf.optimizers.Adam(0.000125 * sdp.size())

@tf.function
def training_step(images, labels, first_batch):
    with tf.GradientTape() as tape:
        probs = mnist_model(images, training=True)
        loss_value = loss(labels, probs)

    # SageMaker data parallel: Wrap tf.GradientTape with the library's DistributedGradientTape
    tape = sdp.DistributedGradientTape(tape)

    grads = tape.gradient(loss_value, mnist_model.trainable_variables)
    opt.apply_gradients(zip(grads, mnist_model.trainable_variables))

    if first_batch:
       # SageMaker data parallel: Broadcast model and optimizer variables
       sdp.broadcast_variables(mnist_model.variables, root_rank=0)
       sdp.broadcast_variables(opt.variables(), root_rank=0)

    return loss_value

...

# SageMaker data parallel: Save checkpoints only from master node.
if sdp.rank() == 0:
    checkpoint.save(checkpoint_dir)
```

For more advanced usage, refer to [SageMaker Distributed Data Parallel TensorFlow API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html#api-documentation)\. 

## Modify a PyTorch Training Script Using SMD Data Parallel<a name="data-parallel-modify-sdp-pt"></a>

The following steps show you how to convert a PyTorch training script to utilize SageMaker's distibuted data parallel library\.

The library APIs are designed to be similar to PyTorch Distributed Data Parallel \(DDP\) APIs\. For additional details on each data parallel API offered for PyTorch, see the [SageMaker distibuted data parallel PyTorch API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html#api-documentation)\. 

1. Import the library’s PyTorch client and initialize it, then import the module for distributed training\. 

   ```
   import smdistributed.dataparallel.torch.distributed as dist
   
   from smdistributed.dataparallel.torch.parallel.distributed import DistributedDataParallel as DDP
   
   dist.init_process_group()
   ```

1. Pin each GPU to a single SageMaker data parallel library process with `local_rank`—this refers to the relative rank of the process within a given node\.

   The `smdistributed.dataparallel.torch.get_local_rank()` API provides you the local rank of the device\. The leader node is rank 0, and the worker nodes are rank 1, 2, 3, and so on\. This is invoked in the next code block as `dist.get_local_rank()`\. 

   ```
   torch.cuda.set_device(dist.get_local_rank())
   ```

1. Wrap the PyTorch model with the library’s DDP\. 

   ```
   model = ...
   # Wrap model with the library's DistributedDataParallel
   model = DDP(model)
   ```

1. Modify the`torch.utils.data.distributed.DistributedSampler` to include the cluster’s information\. Set`num_replicas` to the total number of GPUs participating in training across all the nodes in the cluster\. This is called `world_size`\. You can get `world_size` with the `smdistributed.dataparallel.torch.get_world_size()` API\. This is invoked in the following code as `dist.get_world_size()`\. Also supply the node rank using `smdistributed.dataparallel.torch.get_rank()`\. This is invoked as `dist.get_rank()`\. 

   ```
   train_sampler = DistributedSampler(train_dataset, num_replicas=dist.get_world_size(), rank=dist.get_rank())
   ```

1. Modify your script to save checkpoints only on the leader node\. The leader node has a synchronized model\. This also avoids worker nodes overwriting the checkpoints and possibly corrupting the checkpoints\. 

The following is an example PyTorch training script for distributed training with the library: 

```
# SageMaker data parallel: Import the library PyTorch API
import smdistributed.dataparallel.torch.distributed as dist

# SageMaker data parallel: Import the library PyTorch DDP
from smdistributed.dataparallel.torch.parallel.distributed import DistributedDataParallel as DDP

# SageMaker data parallel: Initialize the library
dist.init_process_group()

class Net(nn.Module):
    ...
    # Define model

def train(...):
    ...
    # Model training

def test(...):
    ...
    # Model evaluation

def main():
    
    # SageMaker data parallel: Scale batch size by world size
    batch_size //= dist.get_world_size() // 8
    batch_size = max(batch_size, 1)

    # Prepare dataset
    train_dataset = torchvision.datasets.MNIST(...)
 
    # SageMaker data parallel: Set num_replicas and rank in DistributedSampler
    train_sampler = torch.utils.data.distributed.DistributedSampler(
            train_dataset,
            num_replicas=dist.get_world_size(),
            rank=dist.get_rank())
 
    train_loader = torch.utils.data.DataLoader(..)
 
    # SageMaker data parallel: Wrap the PyTorch model with the library's DDP
    model = DDP(Net().to(device))
    
    # SageMaker data parallel: Pin each GPU to a single library process.
    torch.cuda.set_device(local_rank)
    model.cuda(local_rank)
    
    # Train
    optimizer = optim.Adadelta(...)
    scheduler = StepLR(...)
    for epoch in range(1, args.epochs + 1):
        train(...)
        if rank == 0:
            test(...)
        scheduler.step()

    # SageMaker data parallel: Save model on master node.
    if dist.get_rank() == 0:
        torch.save(...)

if __name__ == '__main__':
    main()
```

For more advanced usage, see the [SageMaker distributed data parallel PyTorch API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html#api-documentation)\. 

## Launch a Training Job<a name="data-parallel-training"></a>

To launch the training job using your SageMaker distributed data parallel configured training script, you use the `Estimator.fit()` function\. We recommend that you launch a training job using a SageMaker notebook instance or SageMaker Studio\. To see an example of how you can launch a training job using a Studio or a notebook instance, see [Distributed Training Jupyter Notebook Examples](distributed-training-notebook-examples.md)\.

Use the following resources to learn more about using the SageMaker Python SDK with these frameworks:
+ [Use TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html)
+ [Using PyTorch with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html)