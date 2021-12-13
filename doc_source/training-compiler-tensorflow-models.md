# TensorFlow Models<a name="training-compiler-tensorflow-models"></a>

Bring your own TensorFlow model to SageMaker, and run the training job with SageMaker Training Compiler\.

## TensorFlow Models with Hugging Face Transformers<a name="training-compiler-tensorflow-models-transformers"></a>

TensorFlow models with Hugging Face Transformers are based on TensorFlow's [https://www.tensorflow.org/api_docs/python/tf/keras/Model](https://www.tensorflow.org/api_docs/python/tf/keras/Model) API\. Hugging Face Transformers also provide the [https://huggingface.co/transformers/main_classes/trainer.html#transformers.TFTrainer](https://huggingface.co/transformers/main_classes/trainer.html#transformers.TFTrainer) class for TensorFlow to help reduce the effort to configure distributed training, mixed precision, and so on\. After creating your own training script, you can use SageMaker's script mode to directly inject the training script through the SageMaker estimator class and enable SageMaker Training Compiler\.

SageMaker Training Compiler automatically optimizes model training workloads that are built on top of the native TensorFlow API or the high\-level Keras API, such as the TensorFlow transformer models\.

**Topics**
+ [Using Keras](#training-compiler-tensorflow-models-transformers-keras)
+ [Without Keras](#training-compiler-tensorflow-models-transformers-no-keras)

### Using Keras<a name="training-compiler-tensorflow-models-transformers-keras"></a>

For the best compiler acceleration, we recommend using models that are subclasses of TensorFlow Keras \([tf\.keras\.Model](https://www.tensorflow.org/api_docs/python/tf/keras/Model)\)\. As noted in the [Quick tour](https://huggingface.co/transformers/quicktour.html) page in the *Hugging Face Transformers documentation*, you can use the models as regular TensorFlow Keras models\.

#### For single GPU training<a name="training-compiler-tensorflow-models-transformers-keras-single-gpu"></a>

There's no additional change you need to make in the training script\.

#### For distributed training<a name="training-compiler-tensorflow-models-transformers-keras-distributed"></a>

SageMaker Training Compiler acceleration works transparently for multi\-GPU workloads when the model is constructed and trained using Keras APIs within the scope of [https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) call\.

1. Choose the right distributed training strategy\.

   1. For single\-node multi\-GPU, use `tf.distribute.MirroredStrategy` to set the strategy\.

      ```
      strategy = tf.distribute.MirroredStrategy()
      ```

   1. For multi\-node multi\-GPU, add the following code to properly set the TensorFlow distributed training configuration before creating the strategy\.

      ```
      def set_sm_dist_config():
          DEFAULT_PORT = '8890'
          DEFAULT_CONFIG_FILE = '/opt/ml/input/config/resourceconfig.json'
          with open(DEFAULT_CONFIG_FILE) as f:
              config = json.loads(f.read())
              current_host = config['current_host']
          tf_config = {
              'cluster': {
                  'worker': []
              },
              'task': {'type': 'worker', 'index': -1}
          }
          for i, host in enumerate(config['hosts']):
              tf_config['cluster']['worker'].append("%s:%s" % (host, DEFAULT_PORT))
              if current_host == host:
                  tf_config['task']['index'] = i
          os.environ['TF_CONFIG'] = json.dumps(tf_config)
      
      set_sm_dist_config()
      ```

       Use `tf.distribute.MultiWorkerMirroredStrategy` to set the strategy\.

      ```
      strategy = tf.distribute.MultiWorkerMirroredStrategy()
      ```

1. Using the strategy of your choice, wrap the model\.

   ```
   with strategy.scope():
       # create a model and do fit
   ```

### Without Keras<a name="training-compiler-tensorflow-models-transformers-no-keras"></a>

If you want to bring custom models with custom training loops using TensorFlow without Keras, you should wrap the model and the training loop with the TensorFlow function decorator \(`@tf.function`\) to leverage compiler acceleration\.

SageMaker Training Compiler performs a graph\-level optimization, and uses the decorator to make sure your TensorFlow functions are set to run in graph mode\. 

#### For single GPU training<a name="training-compiler-tensorflow-models-transformers-no-keras-single-gpu"></a>

TensorFlow 2\.0 or later has the eager execution on by default, so you should add the `@tf.function` decorator in front of every function that you use for constructing a TensorFlow model\.

#### For distributed training<a name="training-compiler-tensorflow-models-transformers-no-keras-distributed"></a>

In addition to the changes needed for [Using Keras for distributed training](https://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-tensorflow-models.html#training-compiler-tensorflow-models-transformers-keras), you need to ensure that functions to be run on each GPU are annotated with `@tf.function`, while cross\-GPU communication functions are not annotated\. An example training code should look like the following:

```
@tf.function()
def compiled_step(inputs, outputs):
    with tf.GradientTape() as tape:
        pred=model(inputs, training=True)
        total_loss=loss_object(outputs, pred)/args.batch_size
    gradients=tape.gradient(total_loss, model.trainable_variables)
    return total_loss, pred, gradients

def train_step(inputs, outputs):
    total_loss, pred, gradients=compiled_step(inputs, outputs)
    if args.weight_decay > 0.:
        gradients=[g+v*args.weight_decay for g,v in zip(gradients, model.trainable_variables)]

    optimizer.apply_gradients(zip(gradients, model.trainable_variables))

    train_loss.update_state(total_loss)
    train_accuracy.update_state(outputs, pred)

@tf.function()
def train_step_dist(inputs, outputs):
    strategy.run(train_step, args= (inputs, outputs))
```

Note that this instruction can be used for both single\-node multi\-GPU and multi\-node multi\-GPU\.