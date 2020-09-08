# Configure and Save Tensor Data Using the Debugger API Operations<a name="debugger-data"></a>

*Tensors* define the state of a training job at any particular point in its lifecycle\. The Amazon SageMaker Debugger `CollectionConfig` and `DebuggerHookConfig` API operations enable you to control and group tensors into *collections*, and save them in a target S3 bucket\. 

 While constructing a SageMaker estimator, you simply pass your training script through `entry_point` and enable Debugger by adding its hook configuration argument, `debugger_hook_config`\. The following topics include examples of setting up the `debugger_hook_config` using the `CollectionConfig` and `DebuggerHookConfig` API operations to pull tensors out of your training jobs and save\. If you use [Debugger\-supported AWS containers for zero script change](https://docs.aws.amazon.com/sagemaker/latest/dg/train-debugger.html#debugger-supported-aws-containers), you can simply run the training job without changing your training script\. You can also use Debugger for training jobs running in any other [Debugger\-supported AWS containers with script mode](https://docs.aws.amazon.com/sagemaker/latest/dg/train-debugger.html#debugger-supported-aws-containers), making a minimal change of your training script\. 

**Topics**
+ [Tensor Visualization Example Notebooks](#debugger-tensor-visualization-notebooks)
+ [Save Tensors Using Debugger Built\-in Collections](#debugger-save-built-in-collections)
+ [Save Tensors Using Debugger Modified Built\-in Collections](#debugger-save-modified-built-in-collections)
+ [Save Tensors Using Debugger Custom Collections](#debugger-save-custom-collections)
+ [Visualize Tensors Using Amazon SageMaker Debugger TensorBoard Summaries, Distributions, and Histograms](#debugger-enable-tensorboard-summaries)

## Tensor Visualization Example Notebooks<a name="debugger-tensor-visualization-notebooks"></a>

The following two notebook examples show advanced use of Amazon SageMaker Debugger for visualizing tensors\. Debugger provides a transparent view into training deep learning models\.
+ [ Interactive Tensor Analysis in SageMaker Studio Notebook with MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_analysis)

  This notebook example shows how to visualize saved tensors using Amazon SageMaker Debugger\. By visualizing the tensors, you can easily see how the tensor values change while training deep learning algorithms\. This notebook includes a training job with a poorly configured neural network and uses Amazon SageMaker Debugger to aggregate and analyze tensors, including gradients, activation outputs, and weights\. For example, the following plot shows the distribution of gradients of a convolutional layer that is suffering from a vanishing gradient problem\.  
![\[A graph plotting the distribution of gradients of a convolutional layer suffering from a vanishing gradient problem\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger-vanishing-gradient.gif)

  This notebook also illustrates how a good initial hyperparameter setting improves the training process by generating the same tensor distribution plots\. 
+ [ Visualizing and Debugging Tensors from MXNet Model Training](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_plot)

   This notebook example shows how to save and visualize tensors from an MXNet Gluon model training job using Amazon SageMaker Debugger\. It illustrates that Debugger is set to save all tensors to an Amazon S3 bucket and retrieves ReLu activation outputs for the visualization\. The following figure shows a three\-dimensional visualization of the ReLu activation outputs\. The color scheme is set for blue to indicate values close to 0 and yellow to indicate values close to 1\.   
![\[A visualization of the ReLU activation outputs\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/tensorplot.gif)

  In this notebook, the `TensorPlot` class imported from `tensor_plot.py` is designed to plot convolutional neural networks \(CNNs\) that take two\-dimensional images for inputs\. The `tensor_plot.py` script provided with the notebook retrieves tensors using Debugger and visualizes the CNN\. You can run this notebook on SageMaker Studio to reproduce the tensor visualization and implement your own convolutional neural network model to it\. 
+ [Real\-time Tensor Analysis in a SageMaker Notebook with MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_realtime_analysis)

  This example guides you through installing required components for emitting tensors in an Amazon SageMaker training job and using the Debugger API operations to access those tensors while training is running\. A gluon CNN model is trained on the Fashion MNIST dataset\. While the job is running, you will see how Debugger retrieves activation outputs of the first convolutional layer from each of 100 batches and visualizes them\. Also, this will show you how to visualize weights after the job is done\.

## Save Tensors Using Debugger Built\-in Collections<a name="debugger-save-built-in-collections"></a>

You can use built\-in collections of tensors using the `CollectionConfig` API and save them using the `DebuggerHookConfig` API\. The following example shows how to use the default settings of Debugger hook configurations to construct a SageMaker TensorFlow estimator\. You can also utilize this for MXNet, PyTorch, and XGBoost estimators\.

```
import sagemaker
from sagemaker.tensorflow import TensorFlow
from sagemaker.debugger import DebuggerHookConfig, CollectionConfig

# use Debugger CollectionConfig to call built-in collections
collection_configs=[
        CollectionConfig(name="weights"),
        CollectionConfig(name="gradients"),
        CollectionConfig(name="losses"),
        CollectionConfig(name="biases")
        ...
    ]

# configure Debugger hook
# set a target S3 bucket as you want
sagemaker_session = sagemaker.Session()
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'debugger-built-in-collections-hook'

hook_config = DebuggerHookConfig(
    s3_output_path='s3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'.
                    format(BUCKET_NAME=BUCKET_NAME, 
                           LOCATION_IN_BUCKET=LOCATION_IN_BUCKET),
    collection_configs=collection_configs
)

# construct a SageMaker TensorFlow estimator
sagemaker_estimator = TensorFlow(
    entry_point='directory/to/your_training_script.py',
    role=sm.get_execution_role(),
    base_job_name='debugger-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="2.0",
    py_version="py3",
    
    # debugger-specific hook argument below
    debugger_hook_config=hook_config
)

sagemaker_estimator.fit()
```

To see a list of Debugger built\-in collections, see [Debugger Built\-in Collections](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#collection)\.

## Save Tensors Using Debugger Modified Built\-in Collections<a name="debugger-save-modified-built-in-collections"></a>

You can modify the Debugger built\-in collections using the `CollectionConfig` API operation\. The following example shows how to tweak the built\-in `losses` collection and construct a SageMaker TensorFlow estimator\. You can also use this for MXNet, PyTorch, and XGBoost estimators\.

```
import sagemaker
from sagemaker.tensorflow import TensorFlow
from sagemaker.debugger import DebuggerHookConfig, CollectionConfig

# use Debugger CollectionConfig to call and modify built-in collections
collection_configs=[
    CollectionConfig(
                name="losses", 
                parameters={"save_interval": "50"})]

# configure Debugger hook
# set a target S3 bucket as you want
sagemaker_session = sagemaker.Session()
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'debugger-modified-collections-hook'

hook_config = DebuggerHookConfig(
    s3_output_path='s3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'.
                    format(BUCKET_NAME=BUCKET_NAME, 
                           LOCATION_IN_BUCKET=LOCATION_IN_BUCKET),
    collection_configs=collection_configs
)

# construct a SageMaker TensorFlow estimator
sagemaker_estimator = TensorFlow(
    entry_point='directory/to/your_training_script.py',
    role=sm.get_execution_role(),
    base_job_name='debugger-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="2.0",
    py_version="py3",
    
    # debugger-specific hook argument below
    debugger_hook_config=hook_config
)

sagemaker_estimator.fit()
```

For a full list of `CollectionConfig` parameters, see [ Debugger CollectionConfig API](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#configuring-collection-using-sagemaker-python-sdk)\.

## Save Tensors Using Debugger Custom Collections<a name="debugger-save-custom-collections"></a>

You can also save a reduced number of tensors instead of the full set of tensors \(for example, if you want to reduce the amount of data saved in your Amazon S3 bucket\)\. The following example shows how to customize the Debugger hook configuration to specify target tensors that you want to save\. You can use this for TensorFlow, MXNet, PyTorch, and XGBoost estimators\.

```
import sagemaker
from sagemaker.tensorflow import TensorFlow
from sagemaker.debugger import DebuggerHookConfig, CollectionConfig

# use Debugger CollectionConfig to create a custom collection
collection_configs=[
        CollectionConfig(
            name="custom_activations_collection",
            parameters={
                "include_regex": "relu|tanh", # Required
                "reductions": "mean,variance,max,abs_mean,abs_variance,abs_max"
            })
    ]
    
# configure Debugger hook
# set a target S3 bucket as you want
sagemaker_session = sagemaker.Session()
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'debugger-custom-collections-hook'

hook_config = DebuggerHookConfig(
    s3_output_path='s3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'.
                    format(BUCKET_NAME=BUCKET_NAME, 
                           LOCATION_IN_BUCKET=LOCATION_IN_BUCKET),
    collection_configs=collection_configs
)

# construct a SageMaker TensorFlow estimator
sagemaker_estimator = TensorFlow(
    entry_point='directory/to/your_training_script.py',
    role=sm.get_execution_role(),
    base_job_name='debugger-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="2.0",
    py_version="py3",
    
    # debugger-specific hook argument below
    debugger_hook_config=hook_config
)

sagemaker_estimator.fit()
```

For a full list of `CollectionConfig` parameters, see [ Debugger CollectionConfig](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#configuring-collection-using-sagemaker-python-sdk)\.

## Visualize Tensors Using Amazon SageMaker Debugger TensorBoard Summaries, Distributions, and Histograms<a name="debugger-enable-tensorboard-summaries"></a>

Amazon SageMaker Debugger can automatically generate TensorBoard scalar summaries, distributions, and histograms for saved tensors\. You enable this by passing a `TensorBoardOutputConfig` object when you create an `estimator`, as shown in the following example\. You can also choose to disable or enable histograms for individual collections\. By default, the `save_histogram` flag for a collection is set to `True`\. Debugger adds scalar summaries to TensorBoard for all `ScalarCollections` and scalars saved through `hook.save_scalar`\. For more information about scalar collections and the `save_scalar` method, see the [Debugger Hook API](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md)\.

The following example saves weights and gradients as full tensors, and also saves the gradients as histograms and distributions that can be visualized with TensorBoard\. Amazon SageMaker Debugger saves them in the Amazon S3 location passed in the `TensorBoardOutputConfig` object\.

```
import sagemaker
from sagemaker.debugger import DebuggerHookConfig, CollectionConfig, TensorBoardOutputConfig

sagemaker_session = sagemaker.Session()
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'smdebug-hook-tensorboard'

hook_config = DebuggerHookConfig(
    s3_output_path='s3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'.
                    format(BUCKET_NAME=BUCKET_NAME, 
                           LOCATION_IN_BUCKET=LOCATION_IN_BUCKET),
    collection_configs=[
        CollectionConfig(
            name="weights",
            parameters={"save_histogram": "False"}),
        CollectionConfig(name="gradients"),
    ]
)

tb_config = TensorBoardOutputConfig('s3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'.
                                    format(BUCKET_NAME=BUCKET_NAME, 
                                           LOCATION_IN_BUCKET=LOCATION_IN_BUCKET))

from sagemaker.tensorflow import TensorFlow
sagemaker_estimator = TensorFlow(
    entry_point='directory/to/your_training_script.py',
    role=sm.get_execution_role(),
    base_job_name='smdebug-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="2.0",
    py_version="py3",
    
    # debugger-specific arguments below
    debugger_hook_config=hook_config,
    tensorboard_output_config=tb_config
)
sagemaker_estimator.fit(wait=True)
```