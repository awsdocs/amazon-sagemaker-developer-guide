# Considerations for Amazon SageMaker Debugger<a name="debugger-considerations"></a>

Consider the following when using Amazon SageMaker Debugger\.

## Considerations for Distributed Training<a name="debugger-considerations-debug"></a>

The following list shows the scope of validity and considerations for using Debugger on training jobs with deep learning frameworks and various distributed training options\.
+ **Horovod**  
**Scope of validity of using Debugger for training jobs with Horovod**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-considerations.html)
+ **SageMaker distributed data parallel**  
**Scope of validity of using Debugger for training jobs with SageMaker distributed data parallel**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-considerations.html)

  \* Debugger does not support framework profiling for TensorFlow 2\.x\.

  \*\* SageMaker distributed data parallel does not support TensorFlow 2\.x with Keras implementation\.
+ **SageMaker distributed model parallel** – Debugger does not support SageMaker distributed model parallel training\.
+ **Distributed training with SageMaker checkpoints** – Debugger is not available for training jobs when both the distributed training option and SageMaker checkpoints are enabled\. You might see an error that looks like the following: 

  ```
  SMDebug Does Not Currently Support Distributed Training Jobs With Checkpointing Enabled
  ```

  To use Debugger for training jobs with distributed training options, you need to disable SageMaker checkpointing and add manual checkpointing functions to your training script\. For more information about using Debugger with distributed training options and checkpoints, see [Using SageMaker Distributed Data Parallel with SageMaker Debugger and Checkpoints](distributed-troubleshooting-data-parallel.md#distributed-ts-data-parallel-debugger) and [Saving Checkpoints](distributed-troubleshooting-model-parallel.md#distributed-ts-model-parallel-checkpoints)\.
+ **Parameter Server** – Debugger does not support parameter server\-based distributed training\.
+ Profiling distributed training framework operations, such as the `AllReduced` operation of SageMaker distributed data parallel and [Horovod operations](https://horovod.readthedocs.io/en/stable/timeline_include.html), is not available\.

## Considerations for Monitoring System Bottlenecks and Profiling Framework Operations<a name="debugger-considerations-profile"></a>
+ For AWS TensorFlow, data loader metrics cannot be collected using the default `local_path` setting of the `FrameworkProfile` class\. The path has to be manually configured and end in `"/"`\. For example:

  ```
  FrameworkProfile(local_path="/opt/ml/output/profiler/")
  ```
+ For AWS TensorFlow, the data loader profiling configuration cannot be updated while a training job is running\.
+ For AWS TensorFlow, a `NoneType` error might occur when you use analysis tools and notebook examples with TensorFlow 2\.3 training jobs and the detailed profiling option\.
+ Python profiling and detailed profiling are only supported for Keras API\.
+ To access the deep profiling feature for TensorFlow and PyTorch, currently you must specify the latest AWS deep learning container images with CUDA 11\. For example, you must specify the specific image URI in the TensorFlow and PyTorch estimator as follows:
  + For TensorFlow

    ```
    image_uri = f"763104351884.dkr.ecr.{region}.amazonaws.com/tensorflow-training:2.3.1-gpu-py37-cu110-ubuntu18.04"
    ```
  + For PyTorch

    ```
    image_uri = f"763104351884.dkr.ecr.{region}.amazonaws.com/pytorch-training:1.6.0-gpu-py36-cu110-ubuntu18.04"
    ```

## Considerations for Debugging Model Output Tensors<a name="debugger-considerations-debug"></a>
+ Avoid using functional API operations\. Debugger cannot collect model output tensors from PyTorch and MXNet training scripts composed of functional API operations\.
  + Debugger cannot collect model output tensors from the [https://pytorch.org/docs/stable/nn.functional.html](https://pytorch.org/docs/stable/nn.functional.html) API operations\. When you write a PyTorch training script, it is recommended to use the [https://pytorch.org/docs/stable/generated/torch.nn.NLLLoss.html](https://pytorch.org/docs/stable/generated/torch.nn.NLLLoss.html) modules instead\.
  + Debugger cannot collect model output tensors from MXNet functional objects in hybrid blocks\. For example, the ReLu activation \(`F.relu`\) outputs cannot be collected from the following example of [https://mxnet.apache.org/versions/1.8.0/api/python/docs/api/gluon/hybrid_block.html](https://mxnet.apache.org/versions/1.8.0/api/python/docs/api/gluon/hybrid_block.html) with `F` in the `hybrid_forward` function\.

    ```
    import mxnet as mx
    from mxnet.gluon import HybridBlock, nn
    
    class Model(HybridBlock):
        def __init__(self, **kwargs):
            super(Model, self).__init__(**kwargs)
            # use name_scope to give child Blocks appropriate names.
            with self.name_scope():
                self.dense0 = nn.Dense(20)
                self.dense1 = nn.Dense(20)
    
        def hybrid_forward(self, F, x):
            x = F.relu(self.dense0(x))
            return F.relu(self.dense1(x))
    
    model = Model()
    model.initialize(ctx=mx.cpu(0))
    model.hybridize()
    model(mx.nd.zeros((10, 10), ctx=mx.cpu(0)))
    ```