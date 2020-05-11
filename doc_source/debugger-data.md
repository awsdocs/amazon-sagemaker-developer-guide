# Save Tensor Data for Debugger<a name="debugger-data"></a>

*Tensors* define the state of a training job at any particular point in its lifecycle\. To monitor tensors or to save and analyze them to evaluate model training, use the Amazon SageMaker Debugger `smdebug` Python library\. To manage tensors, you can group them into *collections*\. You can use the `DebuggerHookConfig` class in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to save the tensors that you choose at the frequencies you want\. 

**Topics**
+ [Save Built\-in Collections](#debugger-save-built-in-collections)
+ [Save Reductions for a Custom Collection](#debugger-save-reductions-for-custom-collections)
+ [Enable TensorBoard Summaries, Distributions, and Histograms](#debugger-enable-tensorboard-summaries)

## Save Built\-in Collections<a name="debugger-save-built-in-collections"></a>

To learn more about build\-in collections, see [Common API](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md)\.

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

You can define your collection of tensors\. You can also choose to save only certain reductions of tensors instead of the full tensor, for example, to reduce the amount of data saved\. 

**Note**  
If you save reductions, only the reductions are available for analysis\. If you also want to save the raw tensor, pass the `save_raw_tensor` flag, 

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

## Enable TensorBoard Summaries, Distributions, and Histograms<a name="debugger-enable-tensorboard-summaries"></a>

Amazon SageMaker Debugger can automatically generate TensorBoard scalar summaries, distributions, and histograms for saved tensors\. You enable this by passing a `TensorBoardOutputConfig` object when you create an `Estimator`, as shown in the following example\. You can also choose to disable or enable histograms for individual collections\. By default, the `save_histogram` flag for a collection is set to `True`\. Debugger adds scalar summaries to TensorBoard for all `ScalarCollections` and scalars saved through `hook.save_scalar`\. For more information about scalar collections and the `save_scalar` method, see the  [Common API](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md)\.

The following example saves weights and gradients as full tensors, and also saves the gradients as histograms and distributions that can be visualized with TensorBoard\. Amazon SageMaker Debugger saves them in the Amazon S3 location passed in the `TensorBoardOutputConfig` object\.

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