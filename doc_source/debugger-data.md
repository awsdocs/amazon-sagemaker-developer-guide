# Save Tensor Data for Debugger<a name="debugger-data"></a>

Tensors define the state of the training job at any particular instant in its lifecycle\. Amazon SageMaker Debugger provides the `smdebug` library, which allows you to monitor these tensors, save them, and analyze them to evaluate model training\. Tensors can be grouped into collections to help manage them\. Debugger gives you a powerful and flexible API to save the tensors you choose at the frequencies you want\. These configurations are made available in the Amazon SageMaker Python SDK through the `DebuggerHookConfig` class\.

**Topics**
+ [Save Built\-in Party Collections](#debugger-save-built-in-collections)
+ [Save Reductions for a Custom Collection](#debugger-save-reductions-for-custom-collections)
+ [Enable TensorBoard Summaries](#debugger-enable-tensorboard-summaries)

## Save Built\-in Party Collections<a name="debugger-save-built-in-collections"></a>

Learn more about these first party collections [Common API](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md)\.

```
from sagemaker.debugger import DebuggerHookConfig, CollectionConfig
hook_config = DebuggerHookConfig(
    s3_output_path='s3://smdebug-dev-demo-pdx/mnist',
    hook_parameters={
        "save_interval": 100
    },
    collection_configs=[
        CollectionConfig("weights"),
        CollectionConfig("gradients"),
        CollectionConfig("losses"),
        CollectionConfig(
            name="biases",
            parameters={
                "save_interval": 10,
                "end_step": 500
            }
        ),
    ]
)
import sagemaker as sm
sagemaker_estimator = sm.tensorflow.TensorFlow(
    entry_point='src/mnist.py',
    role=sm.get_execution_role(),
    base_job_name='smdebug-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="1.15",
    py_version="py3",
    # smdebug-specific arguments below
    debugger_hook_config=hook_config
)
sagemaker_estimator.fit()
```

## Save Reductions for a Custom Collection<a name="debugger-save-reductions-for-custom-collections"></a>

You can define your collection of tensors\. You can also choose to save certain reductions of tensors only instead of saving the full tensor\. You may choose to do this to reduce the amount of data saved\. 

**Note**  
When you save reductions, unless you pass the flag `save_raw_tensor`, only these reductions will be available for analysis\. The raw tensor will not be saved\.

```
from sagemaker.debugger import DebuggerHookConfig, CollectionConfig
hook_config = DebuggerHookConfig(
    s3_output_path='s3://smdebug-dev-demo-pdx/mnist',
    collection_configs=[
        CollectionConfig(
            name="activations",
            parameters={
                "include_regex": "relu|tanh",
                "reductions": "mean,variance,max,abs_mean,abs_variance,abs_max"
            })
    ]
)
import sagemaker as sm
sagemaker_estimator = sm.tensorflow.TensorFlow(
    entry_point='src/mnist.py',
    role=sm.get_execution_role(),
    base_job_name='smdebug-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="1.15",
    py_version="py3",
    # smdebug-specific arguments below
    debugger_hook_config=hook_config
)
sagemaker_estimator.fit()
```

## Enable TensorBoard Summaries<a name="debugger-enable-tensorboard-summaries"></a>

Amazon SageMaker Debugger can automatically generate TensorBoard scalar summaries, distributions and histograms for tensors saved\. This can be enabled by passing a `TensorBoardOutputConfig` object when creating an `Estimator` as follows\. You can also choose to disable or enable histograms specifically for different collections\. By default a collection has `save_histogram` flag set to `True`\. Note that scalar summaries are added to TensorBoard for all `ScalarCollections` and any scalar saved through `hook.save_scalar`\. For more information on scalar collections and `save_scalar` method, see the [Common API](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md)\.

The following example saves weights and gradients as full tensors, and also saves the gradients as histograms and distributions to visualize in TensorBoard\. These are saved to the location passed in `TensorBoardOutputConfig` object\.

```
from sagemaker.debugger import DebuggerHookConfig, CollectionConfig, TensorBoardOutputConfig
hook_config = DebuggerHookConfig(
    s3_output_path='s3://smdebug-dev-demo-pdx/mnist',
    collection_configs=[
        CollectionConfig(
            name="weights",
            parameters={"save_histogram": False}),
        CollectionConfig(name="gradients"),
    ]
)

tb_config = TensorBoardOutputConfig('s3://smdebug-dev-demo-pdx/mnist/tensorboard')

import sagemaker as sm
sagemaker_estimator = sm.tensorflow.TensorFlow(
    entry_point='src/mnist.py',
    role=sm.get_execution_role(),
    base_job_name='smdebug-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="1.15",
    py_version="py3",
    # smdebug-specific arguments below
    debugger_hook_config=hook_config,
    tensorboard_output_config=tb_config
)
sagemaker_estimator.fit()
```