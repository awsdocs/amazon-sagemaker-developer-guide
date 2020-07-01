# Save Tensor Data Using the Debugger Hook API<a name="debugger-data"></a>

*Tensors* define the state of a training job at any particular point in its lifecycle\. To manage tensors, use Amazon SageMaker Debugger `DebuggerHookConfig` class to group them into *collections*\. 

 The following topics include examples that show how to set up the hook configuration `hook_config` object and include it in an estimator object to run your training script in AWS Deep Learning Containers in a TensorFlow framework\. The argument `entry_point` of `sagemaker_extimator` is where you specify the directory to your training script, and you can simply replace `'directory/to/your_training_script.py'` with your training script without making any changes \(zero script change experience\)\. 

**Topics**
+ [Save Tensors using Built\-in Debugger Collections](#debugger-save-built-in-collections)
+ [Save Reduced Tensors Using Debugger Custom Collections](#debugger-save-reductions-for-custom-collections)
+ [Visualize Tensors using Amazon SageMaker Debugger TensorBoard Summaries, Distributions, and Histograms](#debugger-enable-tensorboard-summaries)
+ [Tensor Visualization Example Notebooks](#debugger-tensor-visualization-notebooks)

## Save Tensors using Built\-in Debugger Collections<a name="debugger-save-built-in-collections"></a>

You can define your collection of tensors using Amazon SageMaker Debugger\.  The following example shows how to use the default setting for Debugger hook configuration for an estimator in a Deep Learning Container with a TensorFlow framework\.

```
import sagemaker
from sagemaker.debugger import DebuggerHookConfig, CollectionConfig

sagemaker_session = sagemaker.Session()
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'smdebug-built-in-collections-hook'

hook_config = DebuggerHookConfig(
    s3_output_path='s3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'.
                    format(BUCKET_NAME=BUCKET_NAME, 
                           LOCATION_IN_BUCKET=LOCATION_IN_BUCKET),
    hook_parameters={
        "save_interval": "100"
    },
    collection_configs=[
        CollectionConfig("weights"),
        CollectionConfig("gradients"),
        CollectionConfig("losses"),
        CollectionConfig(
            name="biases",
            parameters={
                "save_interval": "10",
                "end_step": "500"
            }
        ),
    ]
)
from sagemaker.tensorflow import TensorFlow
sagemaker_estimator = sm.tensorflow.TensorFlow(
    entry_point='directory/to/your_training_script.py',
    role=sm.get_execution_role(),
    base_job_name='smdebug-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="2.0",
    py_version="py3",
    # smdebug-specific arguments below
    debugger_hook_config=hook_config
)

sagemaker_estimator.fit()
```

To see a list of Debugger built\-in collections, see [Debugger Hook API \- Collection](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#collection)\.

## Save Reduced Tensors Using Debugger Custom Collections<a name="debugger-save-reductions-for-custom-collections"></a>

You can also save a reduced number of tensors instead of the full set of tensors; for example, if you want to reduce the amount of data saved in your Amazon S3 bucket\. 

The following example shows how to modify the Debugger hook configuration to specify target tensors that you want to save\.

**Note**  
If you save reductions, only the reductions are available for analysis\. If you also want to save the raw tensor, pass the `save_raw_tensor` flag, 

```
import sagemaker
from sagemaker.debugger import DebuggerHookConfig, CollectionConfig

sagemaker_session = sagemaker.Session()
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'smdebug-reduced-hook'

hook_config = DebuggerHookConfig(
    s3_output_path='s3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'.
                    format(BUCKET_NAME=BUCKET_NAME, 
                           LOCATION_IN_BUCKET=LOCATION_IN_BUCKET),
    collection_configs=[
        CollectionConfig(
            name="activations",
            parameters={
                "include_regex": "relu|tanh",
                "reductions": "mean,variance,max,abs_mean,abs_variance,abs_max"
            })
    ]
)

from sagemaker.tensorflow import TensorFlow
sagemaker_estimator = TensorFlow(
    entry_point='directory/to/your_training_script.py',
    role=sm.get_execution_role(),
    base_job_name='smdebug-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="2.0",
    py_version="py3",
    # smdebug-specific arguments below
    debugger_hook_config=hook_config
)
sagemaker_estimator.fit()
```

## Visualize Tensors using Amazon SageMaker Debugger TensorBoard Summaries, Distributions, and Histograms<a name="debugger-enable-tensorboard-summaries"></a>

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
    # smdebug-specific arguments below
    debugger_hook_config=hook_config,
    tensorboard_output_config=tb_config
)
sagemaker_estimator.fit(wait=True)
```

## Tensor Visualization Example Notebooks<a name="debugger-tensor-visualization-notebooks"></a>

The following two notebook examples show advanced use of Amazon SageMaker Debugger for visualizing tensors\. Debugger provides a completely transparent view into training neural network models\.

**Topics**
+ [[ Interactive Tensor Analysis in SageMaker Studio Notebook with MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_analysis)](#debugger-mnist_tensor_analysis)
+ [[ Visualizing and Debugging Tensors from MXNet Model Training](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_plot)](#debugger-mnist_tensor_plot)

### [ Interactive Tensor Analysis in SageMaker Studio Notebook with MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_analysis)<a name="debugger-mnist_tensor_analysis"></a>

 This notebook example shows how to visualize saved tensors using Amazon SageMaker Debugger\. By visualizing the tensors, you can easily see how the tensor values change while training deep learning algorithms\. This notebook includes a training job with a poorly configured neural network and uses Amazon SageMaker Debugger to aggregate and analyze tensors, including gradients, activation outputs, and weights\. For example, the following plot shows the distribution of gradients of a convolutional layer that is suffering from a vanishing gradient problem\. 

![\[A graph plotting the distribution of gradients of a convolutional layer suffering from a vanishing gradient problem\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger-vanishing-gradient.gif)

This notebook also illustrates how a good initial hyperparameter setting improves the training process by generating the same tensor distribution plots\. 

### [ Visualizing and Debugging Tensors from MXNet Model Training](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_plot)<a name="debugger-mnist_tensor_plot"></a>

 This notebook example shows how to save and visualize tensors from an MXNet Gluon model training job using Amazon SageMaker Debugger\. It illustrates that Debugger is set to save all tensors to an Amazon S3 bucket and retrieves ReLu activation outputs for the visulization\. The following figure shows a three\-dimensional visualization of the ReLu activation outputs\. The color scheme is set for blue to indicate values close to 0 and yellow to indicate values close to 1\. 

![\[A visualization of the ReLU activation outputs\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/tensorplot.gif)

In this notebook, the `TensorPlot` class imported from `tensor_plot.py` is designed to plot convolutional neural networks that take two\-dimentional images for inputs\. The `tensor_plot.py` script provided with the notebook retrieves tensors using Debugger and visualizes the convolutional neural network\. You can run this notebook on Amazon SageMaker Studio to reproduce the tensor visualization and implement your own convolutional neural network model to it\. 