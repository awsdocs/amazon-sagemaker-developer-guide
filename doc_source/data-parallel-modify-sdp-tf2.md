# Modify a TensorFlow Training Script<a name="data-parallel-modify-sdp-tf2"></a>

 The following steps show you how to modify a TensorFlow training script to utilize SageMaker's distributed data parallel library\.  

The library APIs are designed to be similar to Horovod APIs\. For additional details on each API that the library offers for TensorFlow, see the [SageMaker distributed data parallel TensorFlow API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html#api-documentation)\.

**Note**  
SageMaker distributed data parallel is adaptable to TensorFlow training scripts composed of `tf` core modules except `tf.keras` modules\. SageMaker distributed data parallel does not support TensorFlow with Keras implementation\.

**Note**  
SageMaker's distributed data parallelism library supports Automatic Mixed Precision \(AMP\) out of the box\. No extra action is needed to enable AMP other than the framework\-level modifications to your training script\. If gradients are in FP16, the SageMaker data parallelism library runs its `AllReduce` operation in FP16\. For more information about implementing AMP APIs to your training script, see the following resources:  
[Frameworks \- TensorFlow](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html#tensorflow) in the *NVIDIA Deep Learning Performance documentation*
[Automatic Mixed Precision for Deep Learning](https://developer.nvidia.com/automatic-mixed-precision) in the *NVIDIA Developer Docs*
[TensorFlow mixed precision APIs](https://www.tensorflow.org/guide/mixed_precision) in the *TensorFlow documentation*

1. Import the library's TensorFlow client and initialize it\.

   ```
   import smdistributed.dataparallel.tensorflow as sdp 
   sdp.init()
   ```

1. Pin each GPU to a single `smdistributed.dataparallel` process with `local_rank`—this refers to the relative rank of the process within a given node\. The `sdp.tensorflow.local_rank()` API provides you the local rank of the device\. The leader node is rank 0, and the worker nodes are rank 1, 2, 3, and so on\. This is invoked in the following code block as `sdp.local_rank()`\. `set_memory_growth` is not directly related to SageMaker distributed, but must be set for distributed training with TensorFlow\. 

   ```
   gpus = tf.config.experimental.list_physical_devices('GPU')
   for gpu in gpus:
       tf.config.experimental.set_memory_growth(gpu, True)
   if gpus:
       tf.config.experimental.set_visible_devices(gpus[sdp.local_rank()], 'GPU')
   ```

1. Scale the learning rate by the number of workers\. The `sdp.tensorflow.size()` API provides you the number of workers in the cluster\. This is invoked in the following code block as `sdp.size()`\. 

   ```
   learning_rate = learning_rate * sdp.size()
   ```

1. Use the library’s `DistributedGradientTape` to optimize `AllReduce` operations during training\. This wraps `tf.GradientTape`\.  

   ```
   with tf.GradientTape() as tape:
         output = model(input)
         loss_value = loss(label, output)
       
   # SageMaker data parallel: Wrap tf.GradientTape with the library's DistributedGradientTape
   tape = sdp.DistributedGradientTape(tape)
   ```

1. Broadcast the initial model variables from the leader node \(rank 0\) to all the worker nodes \(ranks 1 through n\)\. This is needed to ensure a consistent initialization across all the worker ranks\. Use the `sdp.tensorflow.broadcast_variables` API after the model and optimizer variables are initialized\. This is invoked in the following code block as `sdp.broadcast_variables()`\. 

   ```
   sdp.broadcast_variables(model.variables, root_rank=0)
   sdp.broadcast_variables(opt.variables(), root_rank=0)
   ```

1. Finally, modify your script to save checkpoints only on the leader node\. The leader node has a synchronized model\. This also avoids worker nodes overwriting the checkpoints and possibly corrupting the checkpoints\. 

   ```
   if sdp.rank() == 0:
       checkpoint.save(checkpoint_dir)
   ```

The following is an example TensorFlow training script for distributed training with the library\.

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

After you have completed adapting your training script, move on to [Step 2: Launch a SageMaker Distributed Training Job Using the SageMaker Python SDK](data-parallel-use-api.md)\. 