# Modify a TensorFlow Training Script<a name="model-parallel-customize-training-script-tf"></a>

The following are examples of training scripts that you can use to configure the SageMaker distributed model parallel library with TensorFlow versions 2\.3 with auto\-partitioning and manual partitioning\. This selection of examples also includes an example integrated with Horovod for hybrid model and data parallelism\. We recommend that you review [Unsupported Framework Features](#model-parallel-tf-unsupported-features) before creating a training script\.

The required modifications you must make to your training script to use the library are listed in [TensorFlow 2\.3](#model-parallel-customize-training-script-tf-23)\.

To learn how to modify your training script to use hybrid model and data parallelism with Horovod, see [TensorFlow 2\.3 with Horovod](#model-parallel-customize-training-script-tf-2.3)\.

If you want to use manual partitioning, also review [Manual partitioning with TF 2\.3](#model-parallel-customize-training-script-tf-manual)\. 

**Tip**  
For end to end, runnable notebook examples that demonstrate how to use a TensorFlow training script with the SageMaker distributed model parallel library, see [TensorFlow Examples](distributed-training-notebook-examples.md#distributed-training-notebook-examples-tensorflow)\.

Note that auto\-partitioning is enabled by default\. Unless otherwise specified, the following example scripts use auto\-partitioning\.

**Topics**
+ [Unsupported Framework Features](#model-parallel-tf-unsupported-features)
+ [TensorFlow 2\.3](#model-parallel-customize-training-script-tf-23)
+ [TensorFlow 2\.3 with Horovod](#model-parallel-customize-training-script-tf-2.3)
+ [Manual partitioning with TF 2\.3](#model-parallel-customize-training-script-tf-manual)

## Unsupported Framework Features<a name="model-parallel-tf-unsupported-features"></a>

The following TensorFlow features are unsupported by the library:
+ `tf.GradientTape()` is currently not supported\. You can use `Optimizer.get_gradients()` or `Optimizer.compute_gradients()` instead to compute gradients\.
+ The `tf.train.Checkpoint.restore()` API is currently not supported\. For checkpointing, use `smp.CheckpointManager` instead, which provides the same API and functionality\. Note that checkpoint restores with `smp.CheckpointManager` should take place after the first step\.

## TensorFlow 2\.3<a name="model-parallel-customize-training-script-tf-23"></a>

The following training script changes are required to run a TensorFlow model with SageMaker's distributed model parallel library:

1. Import and initialize the library with [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#smp.init](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#smp.init)\.

1. Define Keras model by inheriting from [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_tensorflow.html](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_tensorflow.html) instead of Keras Model class\. Return the model outputs from the call method of the `smp.DistributedModel` object\. Be mindful that any tensors returned from the call method will be broadcast across model\-parallel devices, incurring communication overhead, so any tensors that are not needed outside the call method \(such as intermediate activations\) should not be returned\.

1. Set `drop_remainder=True` in `tf.Dataset.batch()` method\. This is to ensure that the batch size is always divisible by the number of microbatches\.

1. Seed the random operations in the data pipeline using `smp.dp_rank()`, e\.g\., `shuffle(ds, seed=smp.dp_rank())` to ensure consistency of data samples across GPUs that hold different model partitions\.

1. Put the forward and backward logic in a step function and decorate it with smp\.step\.

1. Perform post\-processing on the outputs across microbatches using [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#StepOutput](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#StepOutput) methods such as reduce\_mean\. The [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#smp.init](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#smp.init) function must have a return value that depends on the output of `smp.DistributedModel`\.

1. If there is an evaluation step, similarly place the forward logic inside an smp\.step\-decorated function and post\-process the outputs using [`StepOutput` API](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#StepOutput)\.

To learn more about the SageMaker's distributed model parallel library API, refer to the [API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\. 

```
import tensorflow as tf

# smdistributed: Import TF2.x API
import smdistributed.modelparallel.tensorflow as smp

# smdistributed: Initialize
smp.init()

# Download and load MNIST dataset.
(x_train, y_train), (x_test, y_test) = tf.keras.datasets.mnist.load_data(
    "MNIST-data-%d" % smp.rank()
)
x_train, x_test = x_train / 255.0, x_test / 255.0

# Add a channels dimension
x_train = x_train[..., tf.newaxis]
x_test = x_test[..., tf.newaxis]

# smdistributed: If needed, seed the shuffle with smp.dp_rank(), and drop_remainder
# in batching to make sure batch size is always divisible by number of microbatches
train_ds = (
    tf.data.Dataset.from_tensor_slices((x_train, y_train))
    .shuffle(10000, seed=smp.dp_rank())
    .batch(256, drop_remainder=True)
)

# smdistributed: Define smp.DistributedModel the same way as Keras sub-classing API 
class MyModel(smp.DistributedModel):
    def __init__(self):
        super(MyModel, self).__init__()
        # defin layers

    def call(self, x, training=None):
        # define forward pass and return the model output

model = MyModel()

loss_object = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
optimizer = tf.keras.optimizers.Adam()
train_accuracy = tf.keras.metrics.SparseCategoricalAccuracy(name="train_accuracy")

# smdistributed: Define smp.step. Return any tensors needed outside
@smp.step
def get_grads(images, labels):
    predictions = model(images, training=True)
    loss = loss_object(labels, predictions)

    grads = optimizer.get_gradients(loss, model.trainable_variables)
    return grads, loss, predictions


@tf.function
def train_step(images, labels):
    gradients, loss, predictions = get_grads(images, labels)

    # smdistributed: Accumulate the gradients across microbatches
    gradients = [g.accumulate() for g in gradients]
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))

    # smdistributed: Merge predictions and average losses across microbatches
    train_accuracy(labels, predictions.merge())
    return loss.reduce_mean()


for epoch in range(5):
    # Reset the metrics at the start of the next epoch
    train_accuracy.reset_states()
    for images, labels in train_ds:
        loss = train_step(images, labels)
    accuracy = train_accuracy.result()
```

## TensorFlow 2\.3 with Horovod<a name="model-parallel-customize-training-script-tf-2.3"></a>

The SageMaker distributed model parallel library can be used with Horovod for hybrid model and data parallelism\. In this case, the total number of GPUs must be divisible by the number of partitions\. The quotient is inferred to be the number of model replicas or data parallelism degree\. For instance, if a training job is launched with 8 processes \(which corresponds to 8 GPUs\), and `partitions` is 2, then the library applies 2\-way model parallelism and 4\-way data parallelism over the 8 GPUs\.

To access the data parallel rank and model parallel rank of a process, you can use `smp.dp_rank()` and `smp.mp_rank()`, respectively\. To see all MPI primitives the library exposes, see [MPI Basics](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/v1.2.0/smd_model_parallel_common_api.html#mpi-basics) in the API documentation\. 

If you are using Horovod, you should not directly call `hvd.init`\. Instead, set `"horovod"` to `True` in the SageMaker Python SDK `modelparallel` parameters, and the library internally initializes Horovod\. This is because Horovod must be initialized based on the device assignments of model partitions, and calling `hvd.init()` directly results in problems\.

Furthermore, using `hvd.DistributedOptimizer` directly results in poor performance or hangs, since this implicitly places the AllReduce operation inside `smp.step`\. The recommended way to use the model parallel library with Horovod is by directly calling `hvd.allreduce` after calling `accumulate()` or `reduce_mean()` on the gradients returned from `smp.step`, as seen in the following example\.

The four main changes needed in the script are:
+ Adding `hvd.allreduce`
+ Broadcasting variables after the first batch, as required by Horovod
+ Setting the `"horovod"` parameter to `True` in the `modelparallel` dict in the Python SDK
+ Seeding shuffling and/or sharding operations in the data pipeline with `smp.dp_rank()`\.

To learn more about the SageMaker's distributed model parallel library API, refer to the [API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\.

```
import tensorflow as tf
import horovod.tensorflow as hvd

# smdistributed: Import TF2.x API 
import smdistributed.modelparallel.tensorflow as smp


# smdistributed: Initialize
smp.init()

# Download and load MNIST dataset.
(x_train, y_train), (x_test, y_test) = tf.keras.datasets.mnist.load_data(
    "MNIST-data-%d" % smp.rank()
)
x_train, x_test = x_train / 255.0, x_test / 255.0

# Add a channels dimension
x_train = x_train[..., tf.newaxis]
x_test = x_test[..., tf.newaxis]

# smdistributed: Seed the shuffle with smp.dp_rank(), and drop_remainder
# in batching to make sure batch size is always divisible by number of microbatches
train_ds = (
    tf.data.Dataset.from_tensor_slices((x_train, y_train))
    .shuffle(10000, seed=smp.dp_rank())
    .batch(256, drop_remainder=True)
)

# smdistributed: Define smp.DistributedModel the same way as Keras sub-classing API 
class MyModel(smp.DistributedModel):
    def __init__(self):
        super(MyModel, self).__init__()
        # define layers

    def call(self, x, training=None):
        # define forward pass and return model outputs


model = MyModel()

loss_object = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
optimizer = tf.keras.optimizers.Adam()
train_accuracy = tf.keras.metrics.SparseCategoricalAccuracy(name="train_accuracy")

# smdistributed: Define smp.step. Return any tensors needed outside
@smp.step
def get_grads(images, labels):
    predictions = model(images, training=True)
    loss = loss_object(labels, predictions)

    grads = optimizer.get_gradients(loss, model.trainable_variables)
    return grads, loss, predictions


@tf.function
def train_step(images, labels, first_batch):
    gradients, loss, predictions = get_grads(images, labels)

    # smdistributed: Accumulate the gradients across microbatches
    # Horovod: Allreduce the accumulated gradients
    gradients = [hvd.allreduce(g.accumulate()) for g in gradients]
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))

    # Horovod: Broadcast the variables after first batch 
    if first_batch:
        hvd.broadcast_variables(model.variables, root_rank=0)
        hvd.broadcast_variables(optimizer.variables(), root_rank=0)

    # smdistributed: Merge predictions across microbatches
    train_accuracy(labels, predictions.merge())
    return loss.reduce_mean()


for epoch in range(5):
    # Reset the metrics at the start of the next epoch
    train_accuracy.reset_states()

    for batch, (images, labels) in enumerate(train_ds):
        loss = train_step(images, labels, tf.constant(batch == 0))
```

## Manual partitioning with TF 2\.3<a name="model-parallel-customize-training-script-tf-manual"></a>

Use `smp.partition` context managers to place operations in specific partition\. Any operation not placed in any `smp.partition` contexts is placed in the `default_partition`\. To learn more about the SageMaker's distributed model parallel library API, refer to the [API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\. 

```
import tensorflow as tf

# smdistributed: Import TF2.x API.
import smdistributed.modelparallel.tensorflow as smp

# smdistributed: Initialize
smp.init()

# Download and load MNIST dataset.
(x_train, y_train), (x_test, y_test) = tf.keras.datasets.mnist.load_data(
    "MNIST-data-%d" % smp.rank()
)
x_train, x_test = x_train / 255.0, x_test / 255.0

# Add a channels dimension
x_train = x_train[..., tf.newaxis]
x_test = x_test[..., tf.newaxis]

# smdistributed: If needed, seed the shuffle with smp.dp_rank(), and drop_remainder
# in batching to make sure batch size is always divisible by number of microbatches.
train_ds = (
    tf.data.Dataset.from_tensor_slices((x_train, y_train))
    .shuffle(10000, seed=smp.dp_rank())
    .batch(256, drop_remainder=True)
)

# smdistributed: Define smp.DistributedModel the same way as Keras sub-classing API.
class MyModel(smp.DistributedModel):
    def __init__(self):
         # define layers

    def call(self, x):
        with smp.partition(0):
            x = self.layer0(x)
        with smp.partition(1):
            return self.layer1(x)


model = MyModel()

loss_object = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
optimizer = tf.keras.optimizers.Adam()
train_accuracy = tf.keras.metrics.SparseCategoricalAccuracy(name="train_accuracy")

# smdistributed: Define smp.step. Return any tensors needed outside
@smp.step
def get_grads(images, labels):
    predictions = model(images, training=True)
    loss = loss_object(labels, predictions)

    grads = optimizer.get_gradients(loss, model.trainable_variables)
    return grads, loss, predictions


@tf.function
def train_step(images, labels):
    gradients, loss, predictions = get_grads(images, labels)

    # smdistributed: Accumulate the gradients across microbatches
    gradients = [g.accumulate() for g in gradients]
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))

    # smdistributed: Merge predictions and average losses across microbatches
    train_accuracy(labels, predictions.merge())
    return loss.reduce_mean()


for epoch in range(5):
    # Reset the metrics at the start of the next epoch
    train_accuracy.reset_states()
    for images, labels in train_ds:
        loss = train_step(images, labels)
    accuracy = train_accuracy.result()
```