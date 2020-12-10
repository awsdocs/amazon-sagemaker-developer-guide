# Modify your training script for SDP<a name="data-parallel-modify-sdp"></a>

## Script modification overview<a name="data-parallel-modify-sdp-overview"></a>
+  SageMaker Python SDK with the SageMaker SDP API: In most cases, all you have to change in your training script is the Horovod or SDP import statements\. You swap these out with SDP’s equivalents\. SDP APIs are designed for ease of use, and to provide seamless integration with existing distributed training toolkits\. 
+  No infrastructure management, you focus on your model training: When training a deep learning model with SDP on SageMaker, you can focus on your model training, while SageMaker does cluster management: brings up the nodes and creates the cluster, completes the training, then tears down the cluster\. 

 To customize your own training script, you will need the following: 
+  You must provide TensorFlow / PyTorch training scripts that are adapted to use SDP\. The following sections provide example code for this\. 
+  Your input data must be in an S3 bucket or in [FSx](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html)in the AWS region that you will use to launch your training job\. If you use the Jupyter notebooks provided, create a SageMaker notebook instance in the same region as the bucket that contains your input data\. For more information about storing your training data, refer to the [SageMaker Python SDK data inputs](https://sagemaker.readthedocs.io/en/stable/overview.html#use-file-systems-as-training-input) documenation\. 

**Tip**  
 Consider using FSx instead of S3 to increase training performance\. It has higher throughput and lower latency than S3\. 

 Use the following sections, to see examples of training scripts that can be used to convert your TensorFlow or PyTorch training scripts\. Then, use one of the example notebooks for your template to launch a training job\. You’ll need to swap your training script with the one that came with the notebook and modify any input functions as necessary\. Once you have launched a training job, you can monitor it using CloudWatch\. 

 Then you can see how to deploy your trained model to an endpoint by following one of the example notebooks for deploying a model\. Finally, you can follow an example notebook to test inference on your deployed model\. 

## Modify a TensorFlow 2\.x training script to use SMD data parallel<a name="data-parallel-modify-sdp-tf2"></a>

 The following steps show you how to convert a TensorFlow 2\.x training script to utilize SDP\.  

 ​ 

 The SDP APIs are designed to be close to Horovod APIs\. Please see the SDP TensorFlow API specification for additional details on each API that SDP offers for TensorFlow\. 
+  First import SDP’s TensorFlow client and initialize it: 

```
import smdistributed.dataparallel.tensorflow as sdp 
sdp.init()
```
+  Pin each GPU to a single smdistributed\.dataparallel process with `local_rank` \- this refers to the relative rank of the process within a given node\. `sdp.tensorflow.local_rank()` API provides you the local rank of the device\. The leader node will be rank 0, and the worker nodes will be rank 1, 2, 3, and so on\. This is invoked in the next code block as `sdp.local_rank()`\. `set_memory_growth` is not directly related to SMD, but must be set for distributed training with TensorFlow\. 

```
gpus = tf.config.experimental.list_physical_devices('GPU')
for gpu in gpus:
    tf.config.experimental.set_memory_growth(gpu, True)
if gpus:
    tf.config.experimental.set_visible_devices(gpus[sdp.local_rank()], 'GPU')
```
+  Scale the learning rate by the number of workers\. `sdp.tensorflow.size()` API provides you number of workers in the cluster\. This is invoked in the next code block as `sdp.size()`\. 

```
learning_rate = learning_rate * sdp.size()
```
+  Use SDP’s `DistributedGradientTape`  to optimize AllReduce operations during training\. This wraps `tf.GradientTape`\.  

```
with tf.GradientTape() as tape:
      output = model(input)
      loss_value = loss(label, output)
    
# SDP: Wrap tf.GradientTape with SDP's DistributedGradientTape
tape = sdp.DistributedGradientTape(tape)
```
+  Broadcast initial model variables from the leader node \(rank 0\) to all the worker nodes \(ranks 1 through n\)\. This is needed to ensure a consistent initialization across all the worker ranks\. For this, you use `sdp.``tensorflow.broadcast_variables` API after the model and optimizer variables are initialized\. This is invoked in the next code block as `sdp.broadcast_variables()`\. 

```
sdp.broadcast_variables(model.variables, root_rank=0)
sdp.broadcast_variables(opt.variables(), root_rank=0)
```
+  Finally, modify your script to save checkpoints only on the leader node\. The leader node will have a synchronized model\. This also avoids worker nodes overwriting the checkpoints and possibly corrupting the checkpoints\. 

```
if sdp.rank() == 0:
    checkpoint.save(checkpoint_dir)
```

 All put together, the following is an example TensorFlow2 training script you will have for distributed training with SDP: 

 ​ 

```
import tensorflow as tf

# SDP: Import SDP TF API
import smdistributed.dataparallel.tensorflow as sdp

# SDP: Initialize SDP
sdp.init()

gpus = tf.config.experimental.list_physical_devices('GPU')
for gpu in gpus:
    tf.config.experimental.set_memory_growth(gpu, True)
if gpus:
    # SDP: Pin GPUs to a single SDP process
    tf.config.experimental.set_visible_devices(gpus[sdp.local_rank()], 'GPU')

# Prepare Dataset
dataset = tf.data.Dataset.from_tensor_slices(...)

# Define Model
mnist_model = tf.keras.Sequential(...)
loss = tf.losses.SparseCategoricalCrossentropy()

# SDP: Scale Learning Rate
# LR for 8 node run : 0.000125
# LR for single node run : 0.001
opt = tf.optimizers.Adam(0.000125 * sdp.size())

@tf.function
def training_step(images, labels, first_batch):
    with tf.GradientTape() as tape:
        probs = mnist_model(images, training=True)
        loss_value = loss(labels, probs)

    # SDP: Wrap tf.GradientTape with SDP's DistributedGradientTape
    tape = sdp.DistributedGradientTape(tape)

    grads = tape.gradient(loss_value, mnist_model.trainable_variables)
    opt.apply_gradients(zip(grads, mnist_model.trainable_variables))

    if first_batch:
       # SDP: Broadcast model and optimizer variables
       sdp.broadcast_variables(mnist_model.variables, root_rank=0)
       sdp.broadcast_variables(opt.variables(), root_rank=0)

    return loss_value

...

# SDP: Save checkpoints only from master node.
if sdp.rank() == 0:
    checkpoint.save(checkpoint_dir)
```

 For more advanced usage, please refer to SageMaker Distributed Data Parallel TensorFlow API documentation\. 

## Modify a PyTorch training script to use SMD data parallel<a name="data-parallel-modify-sdp-pt"></a>

 The following steps show you how to convert a PyTorch training script to utilize SageMaker distibuted data parallel \(SDP\)\.  

 ​ 

 The SDP APIs are designed to be close to PyTorch Distributed Data Parallel \(DDP\) APIs\. Please see SageMaker distibuted data parallel PyTorch API documentation for additional details on each API SDP offers for PyTorch\. 
+  First import SDP’s PyTorch client and initialize it\. You also import the SDP module for distributed training\. 

```
import smdistributed.dataparallel.torch.distributed as dist

from smdistributed.dataparallel.torch.parallel.distributed import DistributedDataParallel as DDP

dist.init_process_group()
```
+  Pin each GPU to a single SDP process with `local_rank` \- this refers to the relative rank of the process within a given node\. `smdistributed.dataparallel.torch.get_local_rank()` API provides you the local rank of the device\. The leader node will be rank 0, and the worker nodes will be rank 1, 2, 3, and so on\. This is invoked in the next code block as `dist.get_local_rank()`\. 

```
torch.cuda.set_device(dist.get_local_rank())
```
+  Then wrap the PyTorch model with SDP’s DDP\. 

```
model = ...
# Wrap model with SDP DistributedDataParallel
model = DDP(model)
```
+  Modify the`torch.utils.data.distributed.DistributedSampler` to include the cluster’s information\. Set`num_replicas` to the total number of GPUs participating in training across all the nodes in the cluster\. This is called `world_size`\. You can get `world_size` with `smdistributed.dataparallel.torch.get_world_size()` API\. This is invoked in the following code as `dist.get_world_size()`\. Also supply the node rank using `smdistributed.dataparallel.torch.get_rank()`\. This is invoked as `dist.get_rank()`\. 

```
train_sampler = DistributedSampler(train_dataset, num_replicas=dist.get_world_size(), rank=dist.get_rank())
```
+  Finally, modify your script to save checkpoints only on the leader node\. The leader node will have a synchronized model\. This also avoids worker nodes overwriting the checkpoints and possibly corrupting the checkpoints\. 
+  if dist\.get\_rank\(\) == 0:     \# Save checkpoints 

 ​ 

 All put together, the following is an example PyTorch training script you will have for distributed training with SDP: 

 ​ 

```
# SDP: Import SDP PyTorch API
import smdistributed.dataparallel.torch.distributed as dist

# SDP: Import SDP PyTorch DDP
from smdistributed.dataparallel.torch.parallel.distributed import DistributedDataParallel as DDP

# SDP: Initialize SDP
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
    
    # SDP: Scale batch size by world size
    batch_size //= dist.get_world_size() // 8
    batch_size = max(batch_size, 1)

    # Prepare dataset
    train_dataset = torchvision.datasets.MNIST(...)
 
    # SDP: Set num_replicas and rank in DistributedSampler
    train_sampler = torch.utils.data.distributed.DistributedSampler(
            train_dataset,
            num_replicas=dist.get_world_size(),
            rank=dist.get_rank())
 
    train_loader = torch.utils.data.DataLoader(..)
 
    # SDP: Wrap the PyTorch model with SDP’s DDP
    model = DDP(Net().to(device))
    
    # SDP: Pin each GPU to a single SDP process.
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

    # SDP: Save model on master node.
    if dist.get_rank() == 0:
        torch.save(...)

if __name__ == '__main__':
    main()
```

 For more advanced usage, please refer to SageMaker Distributed Data Parallel PyTorch API documentation\. 

## Launch a training job<a name="data-parallel-training"></a>

To launch the training job using your SDP configured training script, you use the `Estimator.fit()` function\. It is recommended to launch a training job using a SageMaker notebook instance or SageMaker Studio\.
+ To see an example of how you can launch a training job using a Studio or a notebook instance, see [Distributed Training Jupyter Notebok Examples](distributed-training-notebook-examples.md)\.

Use the following resources to learn more about using the SageMaker Python SDK with these frameworks:
+ [Use TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html)
+ [Using PyTorch with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html)