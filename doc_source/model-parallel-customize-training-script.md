# Modify Your Training Script to Use SMP<a name="model-parallel-customize-training-script"></a>

Use this page to learn how to customize your training script with SMP specific API functions and parameters\. To learn more about the SMP API, refer to [SMP API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\. 

## Modify a TensorFlow Training Script<a name="model-parallel-customize-training-script-tf"></a>

The following are examples of training scripts that you can use to configure SMP with TensorFlow version 2\.3, with auto\-partitioning and manual partitioning\. It also includes an example integrated with Horovod, for hybrid model and data parallelism\. It is recommended that you review [Unsupported Framework Features](#model-parallel-tf-unsupported-features) before creating a training script\.

Note that auto\-partitioning enabled by default\. Unless otherwise specified, the following scripts use auto\-partitioning\. 

**Topics**
+ [Unsupported Framework Features](#model-parallel-tf-unsupported-features)
+ [TensorFlow 2\.3](#model-parallel-customize-training-script-tf-23)
+ [TensorFlow 2\.3 with Horovod](#model-parallel-customize-training-script-tf-2.3)
+ [Manual partitioning with TF 2\.3](#model-parallel-customize-training-script-tf-manual)

### Unsupported Framework Features<a name="model-parallel-tf-unsupported-features"></a>

The following TensorFlow features are unsupported by SMP:
+ `tf.GradientTape()` is currently not supported\. You can use `Optimizer.get_gradients()` or `Optimizer.compute_gradients()` instead to compute gradients\.
+ `tf.train.Checkpoint.restore()` API is currently not supported\. For checkpointing, use `smp.CheckpointManager` instead, which provides the same API and functionality\. Note that checkpoint restores with `smp.CheckpointManager` should take place after the first step\.

### TensorFlow 2\.3<a name="model-parallel-customize-training-script-tf-23"></a>

```
# SMP: Import TF2 API
import smdistributed.modelparallel.tensorflow as smp

smp.init()

# SMP: Define the DistributedModel
class Model(smp.DistributedModel):
    def __init__(self):
         # define layers

    def call(self, x):
        # define forward pass

loss_obj = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)

# SMP: If the last batch will not be divisible by num. microbatches,
# set drop_remainder=True.
train_ds = tf.data.Dataset.from_tensor_slices(...).batch(128, drop_remainder=True)

# SMP: Define the smp.step consisting of forward and backward passes
@smp.step
def forward_backward(images, labels):
    predictions = model(images, training=True)
    loss = loss_obj(labels, predictions)

    grads = optimizer.get_gradients(loss, model.trainable_variables)

    return grads, loss

@tf.function
def train_step(images, labels):
    gradients, loss = forward_backward(images, labels)

    # SMP: accumulate the gradients across microbatches
    gradients = [g.accumulate() for g in gradients]
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))

    # SMP: Average the loss across microbatches
    return loss.reduce_mean()

for images, labels in train_ds:
    loss = train_step(images, labels)

    # SMP: Only print loss at rank 0.
    if smp.rank() == 0:
        print(f"Loss: {loss}")
```

### TensorFlow 2\.3 with Horovod<a name="model-parallel-customize-training-script-tf-2.3"></a>

SMP can be used with Horovod for hybrid model/data parallelism\. In this case, the total number of GPUs must be divisible by the number of partitions\. The quotient is inferred to be the number of model replicas, or data parallelism degree\. For instance, if SMP is launched with 8 processes \(which corresponds to 8 GPUs\), and `partitions` is 2, then SMP applies 2\-way model parallelism and 4\-way data parallelism over the 8 GPUs\. To access the data\-parallel\-rank and model\-parallel\-rank of a process, you can use `smp.dp_rank()` and `smp.mp_rank()`, respectively\.

If Horovod is used, `hvd.init` should not be called directly\. Instead, you can set `"horovod"` to `True` in SageMaker Python SDK `modelparallel` parameters, and SMP internally initializes Horovod\. This is because Horovod must be initialized based on the device assignments of model partitions, and calling `hvd.init()` directly will result in problems\.

Furthermore, using `hvd.DistributedOptimizer` directly results in poor performance or hangs, since this implicitly places the allreduce operation inside `smp.step`\. The recommended way to use SMP with Horovod is by directly calling `hvd.allreduce` after calling `accumulate()` or `reduce_mean()` on the gradients returned from `smp.step`, as seen in the following example\.

The four main changes needed in the script are:
+ Adding `hvd.allreduce`
+ Broadcasting variables after first batch, as required by Horovod
+ Setting `"horovod"` parameter to `True` in `modelparallel` dict in Python SDK
+ Seeding shuffling and/or sharding operations in the data pipeline with `smp.dp_rank()`\.

```
# SMP: Import TF2 API
import smdistributed.modelparallel.tensorflow as smp
import horovod.tensorflow as hvd

# SMP: Initialize.
smp.init()

# SMP: Define the DistributedModel
class Model(smp.DistributedModel):
    def __init__(self):
         # define layers

    def call(self, x):
        # define forward pass

loss_obj = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
train_ds = tf.data.Dataset.from_tensor_slices(...)

# SMP: Seed the randomness in data pipeline with smp.dp_rank()
train_ds = train_ds.shuffle(10000, seed=smp.dp_rank())

# SMP: If the last batch will not be divisible by num. microbatches,
# set drop_remainder=True.
train_ds = train_ds.batch(128, drop_remainder=True)


# SMP: Define the smp.step consisting of forward and backward passes
@smp.step
def forward_backward(images, labels):
    predictions = model(images, training=True)
    loss = loss_obj(labels, predictions)

    grads = optimizer.get_gradients(loss, model.trainable_variables)

    return grads, loss

@tf.function
def train_step(images, labels, first_batch):
    gradients, loss = forward_backward(images, labels)

    # SMP: Accumulate the gradients across microbatches.
    # Horovod: allreduce gradients across model replicas.
    gradients = [hvd.allreduce(g.accumulate()) for g in gradients]
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))

    # Horovod: Broadcast variables.
    if first_batch:
        hvd.broadcast_variables(model.variables, root_rank=0)
        hvd.broadcast_variables(optimizer.variables(), root_rank=0)

    # SMP: Average the loss across microbatches.
    return loss.reduce_mean()

for step, (images, labels) in enumerate(train_ds):
    loss = train_step(images, labels, step == 0)

    # SMP: Only print loss at rank 0.
    if smp.rank() == 0:
        print(f"Loss: {loss}")
```

### Manual partitioning with TF 2\.3<a name="model-parallel-customize-training-script-tf-manual"></a>

Use `smp.partition` context managers to place operations in specific partition\. Any operation not placed in any `smp.partition` contexts will be placed in the `default_partition`\.

```
# SMP: Import TF2 API.
import smdistributed.modelparallel.tensorflow as smp

# SMP: Initialize
smp.init()

# SMP: Define the DistributedModel
class Model(smp.DistributedModel):
    def __init__(self):
         # define layers

    def call(self, x):
        with smp.partition(0):
            x = self.layer0(x)
        with smp.partition(1):
            return self.layer1(x)

loss_obj = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)

# SMP: If the last batch will not be divisible by num. microbatches,
# set drop_remainder=True.
train_ds = tf.data.Dataset.from_tensor_slices(...).batch(128, drop_remainder=True)

# SMP: Define the smp.step consisting of forward and backward passes.
@smp.step
def forward_backward(images, labels):
    predictions = model(images, training=True)
    loss = loss_obj(labels, predictions)

    grads = optimizer.get_gradients(loss, model.trainable_variables)

    return grads, loss

@tf.function
def train_step(images, labels):
    gradients, loss = forward_backward(images, labels)

    # SMP: Accumulate the gradients across microbatches.
    gradients = [g.accumulate() for g in gradients]
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))

    # SMP: Average the loss across microbatches.
    return loss.reduce_mean()

for images, labels in train_ds:
    loss = train_step(images, labels)

    # SMP: Only print loss at rank 0.
    if smp.rank() == 0:
        print(f"Loss: {loss}")
```

## Modify a PyTorch Training Script<a name="model-parallel-customize-training-script-pt"></a>

The following are examples of training scripts that you can use to configure SMP with PyTorch version 1\.6, with auto\-partitioning and manual partitioning\. It is recommended that you review the [Important Considerations](#model-parallel-pt-considerations) and [Unsupported Framework Features](#model-parallel-pt-unsupported-features) before creating a training script\.

Note that auto\-partitioning enabled by default\. Unless otherwise specified, the following scripts use auto\-partitioning\. 

**Topics**
+ [Important Considerations](#model-parallel-pt-considerations)
+ [Unsupported Framework Features](#model-parallel-pt-unsupported-features)
+ [PyTorch 1\.6\.0](#model-parallel-customize-training-script-pt-16)
+ [Manual Partitioning with PyTorch 1\.6\.0](#model-parallel-customize-training-script-pt-16-hvd)

### Important Considerations<a name="model-parallel-pt-considerations"></a>

When you configure a PyTorch training script, you should be aware of the following:
+ If you are using an optimization technique that relies on global gradient norms, for example gradient norm from the entire model, such as some variants of LAMB optimizer or global gradient clipping, you need to gather all the norms across the model partitions for correctness\. You can use SMP’s Communication primitives to do this\.
+ All `torch.Tensor` arguments to the forward methods of the `nn.Modules` in your model must be used in the computation of the module output\. In other words, SMP does not support the case where there is a torch\. Tensor argument to a module that the module output does not depend on\.
+ The argument to the `smp.DistributedModel.backward()` call must depend on all model outputs\. In other words, there cannot be an output from the `smp.DistributedModel.forward` call that is not used in the computation of the tensor that is fed into `smp.DistributedModel.backward` call\.
+ If there are `torch.cuda.synchronize()` calls in your code, you might need to call `torch.cuda.set_device(smp.local_rank())` immediately before the synchronize call\. Otherwise unnecessary CUDA contexts might be created in device 0, which will needlessly consume memory\.
+ Since SMP places `nn.Modules` on different devices, the modules in the model must not depend on any global state that is modified inside `smp.step`\. Any state that remains fixed throughout training, or that is modified outside `smp.step` in a way that is visible to all processes is allowed\.
+ You don’t need to move the model to GPU \(using \.`to()` API\) when using SMP\. If you try to move the model to GPU before the model is partitioned \(before the first `smp.step` call\), the move call will be ignored\. SMP automatically moves the part of the model assigned to a rank to its GPU\. Once training with SMP starts, don’t move the model to CPU and use it, as it won’t have correct parameters for modules not assigned to the partition held by the process\. If you want to use SMP trained model without SMP later, such as for inference, the recommended way is to save the full model using our checkpointing API and load it back to a regular PyTorch Module\.
+ If you have a list of modules such that output of one feeds into another, replacing that list with `nn.Sequential` can significantly improve performance\.
+ The weight update \(`optimizer.step()`\) needs to happen outside of `smp.step` because that is when the entire backward pass is done and gradients are ready\. When using a hybrid model with model and data parallelism, at this point, allreduce of gradients is also guaranteed to finish\.
+ When using SMP in combination with data parallelism, make sure that the number of batches on all data parallel ranks is the same so that allreduce does not hang, waiting for a rank which is not participating in the step\.
+ The inputs to `smp.step` must be the model inputs generated by `DataLoader`\. This is because `smp.step` internally splits the input tensors along the batch dimension and pipelines them\. This means that passing DataLoader itself to the `smp.step` function to generate the model inputs inside will not work\.
+ The input tensors to `smp.step` must be moved to the current device using `.to()` API, which must take place after the `torch.cuda.set_device(local_rank())` call\.

### Unsupported Framework Features<a name="model-parallel-pt-unsupported-features"></a>

The following PyTorch features are unsupported by SMP:
+ When using data parallelism with DDP, the `DistributedDataParallel` wrapper is not supported\. SMP internally manages integrating with DDP, including parameter broadcast and gradient allreduce\. Please note that with SMP, module buffers are only broadcast once at the start of training\. If your model has module buffers and you require module buffers to be synchronized across data parallel groups at each step, you can do so through the `torch.distributed` API, using the process group that can be obtained via `smp.get_dp_process_group()`\.
+ For mixed precision training, apex\.amp module is not supported\. The recommended way to use SMP with automatic mixed\-precision is to use `torch.cuda.amp`, with the exception of using `smp.amp.GradScaler` instead of the implementation in torch\.
+ `torch.jit.ScriptModules` or `ScriptFunctions` are not supported by `smp.DistributedModel`\.
+ `apex` : `FusedLayerNorm`, `FusedAdam`, `FusedLAMB`, and `FusedNovoGrad` from `apex` are not supported\. You can use the SMP implementations of these through `smp.optimizers` and `smp.nn` APIs, instead\.

### PyTorch 1\.6\.0<a name="model-parallel-customize-training-script-pt-16"></a>

The following changes are required to run a PyTorch model with SMP:

1. Import and initialize SMP with `smp.init()`\.

1. Wrap the model with `smp.DistributedModel`\.

1. Wrap optimizer with `smp.DistributedOptimizer`\.

1. Use the returned `DistributedModel` object instead of user model\.

1. Put the forward and backward logic in a step function and decorate it with `smp.step`\.

1. Restrict each process to its own device through `torch.cuda.set_device(smp.local_rank())`\.

1. Move input tensors to GPU using the `.to()` API before the `smp.step` call \(see example below\)\.

1. Replace `torch.Tensor.backward` and `torch.autograd.backward` with `DistributedModel.backward`\.

1. Post\-processing on the outputs across microbatches using `StepOutput` methods such as `reduce_mean`\.

```
# SMP: Import SMP API
import smdistributed.modelparallel.torch as smp

# SMP: Initialize SMP.
smp.init()

class Model(nn.Module):
    def __init__(self):
        # define child modules

    def forward(self, x):
        # define forward pass

model = Model()

# Download dataset and create dataloader
dataset2 = datasets.MNIST("../data", train=False, transform=transform)
train_loader = torch.utils.data.DataLoader(dataset1, drop_last=True, **kwargs)

# SMP: Instantiate DistributedModel object using the model.
# This handles distributing the model among multiple ranks
# behind the scenes.
# If ddp is enabled this will take care of broadcasting parameters
# and does overlapping_all_reduce by default.
model = smp.DistributedModel(model)

# SMP: Define the smp.step consisting of forward and backward passes.
@smp.step()
def forward_backward(model, images, labels):
    loss = model(images, labels)
    # SMP: Call backward on the model instead of the output tensor.
    # If loss is not a scalar or a 0d tensor, the backward call requires
    # out grad tensors in addition to the output tensors,
    # similar to torch.autograd.backward call.
    model.backward(loss)
    return loss

for batch_idx, (image, label) in enumerate(train_loader):
    image, label = image.to(device), label.to(device)
    optimizer.zero_grad()
    loss_mb = forward_backward(model, image, label)

    # SMP: average the loss across microbatches
    loss = loss_mb.reduce_mean()
    optimizer.step()

    # SMP: print the loss only at rank 0
    if smp.rank() == 0:
        print(f"Loss: {loss}")
```

### Manual Partitioning with PyTorch 1\.6\.0<a name="model-parallel-customize-training-script-pt-16-hvd"></a>

Use `smp.partition` context managers to place modules in specific devices\. Any module not placed in any `smp.partition` contexts will be placed in the `default_partition`\. `default_partition` needs to be provided if `auto_partition` is set to False\. The modules that are created within a specific `smp.partition` context will be placed on the corresponding partition\.

```
# SMP: Import SMP API.
import smdistributed.modelparallel.torch as smp

# SMP: Initialize SMP
smp.init()

class Model(nn.Module):
    def __init__(self):
        with smp.partition(0):
            # define child modules on device 0
        with smp.partition(1):
            # define child modules on device 1
    def forward(self, x):
        # define forward pass

model = Model()

# Download dataset and create dataloader.
dataset2 = datasets.MNIST("../data", train=False, transform=transform)
train_loader = torch.utils.data.DataLoader(dataset1, drop_last=True, **kwargs)

# SMP: Instantiate DistributedModel object using the model.
# This handles distributing the model among multiple ranks
# behind the scenes.
# If horovod is enabled this will do an overlapping_all_reduce by
# default.
model = smp.DistributedModel(model)

# SMP: Define the smp.step consisting of forward and backward passes.
@smp.step()
def forward_backward(model, images, labels):
    loss = model(images, labels)
    # SMP: Call backward on the model instead of the output tensor.
    # If loss is not a scalar or a 0d tensor, the backward call requires
    # out grad tensors in addition to the output tensors,
    # similar to torch.autograd.backward call.
    model.backward(loss)
    return loss

for batch_idx, (image, label) in enumerate(train_loader):
    # SMP: move the inputs to the current device
    image, label = image.to(device), label.to(device)

    optimizer.zero_grad()
    loss_mb = forward_backward(model, images, labels)

    # SMP: Average the loss across microbatches.
    loss = loss_mb.reduce_mean()
    optimizer.step()

    # SMP: Print the loss only at rank 0.
    if smp.rank() == 0:
        print(f"Loss: {loss}")
```