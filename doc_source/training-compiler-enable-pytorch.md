# Run PyTorch Training Jobs with SageMaker Training Compiler<a name="training-compiler-enable-pytorch"></a>

You can use any of the SageMaker interfaces to run a training job with SageMaker Training Compiler: Amazon SageMaker Studio, Amazon SageMaker notebook instances, AWS SDK for Python \(Boto3\), and AWS Command Line Interface\.

**Topics**
+ [Using the SageMaker Python SDK](#training-compiler-enable-pytorch-pysdk)
+ [Using the SageMaker `CreateTrainingJob` API Operation](#training-compiler-enable-pytorch-api)

## Using the SageMaker Python SDK<a name="training-compiler-enable-pytorch-pysdk"></a>

SageMaker Training Compiler for PyTorch is available through the SageMaker [https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html) and [https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/sagemaker.huggingface.html#hugging-face-estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/sagemaker.huggingface.html#hugging-face-estimator) framework estimator classes\. To turn on SageMaker Training Compiler, add the `compiler_config` parameter to the SageMaker estimators\. Import the `TrainingCompilerConfig` class and pass an instance of it to the `compiler_config` parameter\. The following code examples show the structure of SageMaker estimator classes with SageMaker Training Compiler turned on\.

**Tip**  
To get started with prebuilt models provided by PyTorch or Transformers, try using the batch sizes provided in the reference table at [Tested Models](training-compiler-support.md#training-compiler-tested-models)\.

**Note**  
The native PyTorch support is available in the SageMaker Python SDK v2\.121\.0 and later\. Make sure that you update the SageMaker Python SDK accordingly\.

**Note**  
Starting PyTorch v1\.12\.0, SageMaker Training Compiler containers for PyTorch are available\. Note that the SageMaker Training Compiler containers for PyTorch are not prepackaged with Hugging Face Transformers\. If you need to install the library in the container, make sure that you add the `requirements.txt` file under the source directory when submitting a training job\.  
For PyTorch v1\.11\.0 and before, use the previous versions of the SageMaker Training Compiler containers for Hugging Face and PyTorch\.  
For a complete list of framework versions and corresponding container information, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.

For information that fits your use case, see one of the following options\.

### For single GPU training<a name="training-compiler-estimator-pytorch-single"></a>

------
#### [ PyTorch v1\.12\.0 and later ]

To compile and train a PyTorch model, configure a SageMaker PyTorch estimator with SageMaker Training Compiler as shown in the following code example\.

**Note**  
This native PyTorch support is available in the SageMaker Python SDK v2\.120\.0 and later\. Make sure that you update the SageMaker Python SDK\.

```
from sagemaker.pytorch import PyTorch, TrainingCompilerConfig

# the original max batch size that can fit into GPU memory without compiler
batch_size_native=12
learning_rate_native=float('5e-5')

# an updated max batch size that can fit into GPU memory with compiler
batch_size=64

# update learning rate
learning_rate=learning_rate_native/batch_size_native*batch_size

hyperparameters={
    "n_gpus": 1,
    "batch_size": batch_size,
    "learning_rate": learning_rate
}

pytorch_estimator=PyTorch(
    entry_point='train.py',
    source_dir='path-to-requirements-file', # Optional. Add this if need to install additional packages.
    instance_count=1,
    instance_type='ml.p3.2xlarge',
    framework_version='1.13.1',
    py_version='py3',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

pytorch_estimator.fit()
```

------
#### [ Hugging Face Transformers with PyTorch v1\.11\.0 and before ]

To compile and train a transformer model with PyTorch, configure a SageMaker Hugging Face estimator with SageMaker Training Compiler as shown in the following code example\.

```
from sagemaker.huggingface import HuggingFace, TrainingCompilerConfig

# the original max batch size that can fit into GPU memory without compiler
batch_size_native=12
learning_rate_native=float('5e-5')

# an updated max batch size that can fit into GPU memory with compiler
batch_size=64

# update learning rate
learning_rate=learning_rate_native/batch_size_native*batch_size

hyperparameters={
    "n_gpus": 1,
    "batch_size": batch_size,
    "learning_rate": learning_rate
}

pytorch_huggingface_estimator=HuggingFace(
    entry_point='train.py',
    instance_count=1,
    instance_type='ml.p3.2xlarge',
    transformers_version='4.21.1',
    pytorch_version='1.11.0',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

pytorch_huggingface_estimator.fit()
```

To prepare your training script, see the following pages\.
+ [For single GPU training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-transformers-trainer-single-gpu) of a PyTorch model using Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)
+ [For single GPU training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer-single-gpu) of a PyTorch model without Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)

To find end\-to\-end examples, see the following notebooks:
+ [Compile and Train a Hugging Face Transformers Trainer Model for Question and Answering with the SQuAD dataset ](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_single_gpu_single_node/albert-base-v2/albert-base-v2.html) 
+ [Compile and Train a Hugging Face Transformer `BERT` Model with the SST Dataset using SageMaker Training Compiler](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_single_gpu_single_node/bert-base-cased/bert-base-cased-single-node-single-gpu.html) 
+ [Compile and Train a Binary Classification Trainer Model with the SST2 Dataset for Single\-Node Single\-GPU Training ](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_single_gpu_single_node/roberta-base/roberta-base.html)

------

### For distributed training<a name="training-compiler-estimator-pytorch-distributed"></a>

------
#### [ PyTorch v1\.12 ]

For PyTorch v1\.12, you can run distributed training with SageMaker Training Compiler by adding the `pytorch_xla` option specified to the `distribution` parameter of the SageMaker PyTorch estimator class\.

**Note**  
This native PyTorch support is available in the SageMaker Python SDK v2\.121\.0 and later\. Make sure that you update the SageMaker Python SDK\.

```
from sagemaker.pytorch import PyTorch, TrainingCompilerConfig

# choose an instance type, specify the number of instances you want to use,
# and set the num_gpus variable the number of GPUs per instance.
instance_count=1
instance_type='ml.p3.8xlarge'
num_gpus=4

# the original max batch size that can fit to GPU memory without compiler
batch_size_native=16
learning_rate_native=float('5e-5')

# an updated max batch size that can fit to GPU memory with compiler
batch_size=26

# update learning rate
learning_rate=learning_rate_native/batch_size_native*batch_size*num_gpus*instance_count

hyperparameters={
    "n_gpus": num_gpus,
    "batch_size": batch_size,
    "learning_rate": learning_rate
}

pytorch_estimator=PyTorch(
    entry_point='your_training_script.py',
    source_dir='path-to-requirements-file', # Optional. Add this if need to install additional packages.
    instance_count=instance_count,
    instance_type=instance_type,
    framework_version='1.13.1',
    py_version='py3',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    distribution ={'pytorchxla' : { 'enabled': True }},
    disable_profiler=True,
    debugger_hook_config=False
)

pytorch_estimator.fit()
```

**Tip**  
To prepare your training script, see [PyTorch](training-compiler-pytorch-models.md)

------
#### [ Transformers v4\.21 with PyTorch v1\.11 ]

For PyTorch v1\.11 and later, SageMaker Training Compiler is available for distributed training with the `pytorch_xla` option specified to the `distribution` parameter\.

```
from sagemaker.huggingface import HuggingFace, TrainingCompilerConfig

# choose an instance type, specify the number of instances you want to use,
# and set the num_gpus variable the number of GPUs per instance.
instance_count=1
instance_type='ml.p3.8xlarge'
num_gpus=4

# the original max batch size that can fit to GPU memory without compiler
batch_size_native=16
learning_rate_native=float('5e-5')

# an updated max batch size that can fit to GPU memory with compiler
batch_size=26

# update learning rate
learning_rate=learning_rate_native/batch_size_native*batch_size*num_gpus*instance_count

hyperparameters={
    "n_gpus": num_gpus,
    "batch_size": batch_size,
    "learning_rate": learning_rate
}

pytorch_huggingface_estimator=HuggingFace(
    entry_point='your_training_script.py',
    instance_count=instance_count,
    instance_type=instance_type,
    transformers_version='4.21.1',
    pytorch_version='1.11.0',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    distribution ={'pytorchxla' : { 'enabled': True }},
    disable_profiler=True,
    debugger_hook_config=False
)

pytorch_huggingface_estimator.fit()
```

**Tip**  
To prepare your training script, see the following pages\.  
[For distributed training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-transformers-trainer-distributed) of a PyTorch model using Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)
[For distributed training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer-distributed) of a PyTorch model without Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)

------
#### [ Transformers v4\.17 with PyTorch v1\.10\.2 and before ]

For the supported version of PyTorch v1\.10\.2 and before, SageMaker Training Compiler requires an alternate mechanism for launching a distributed training job\. To run distributed training, SageMaker Training Compiler requires you to pass a SageMaker distributed training launcher script to the `entry_point` argument, and pass your training script to the `hyperparameters` argument\. The following code example shows how to configure a SageMaker Hugging Face estimator applying the required changes\.

```
from sagemaker.huggingface import HuggingFace, TrainingCompilerConfig

# choose an instance type, specify the number of instances you want to use,
# and set the num_gpus variable the number of GPUs per instance.
instance_count=1
instance_type='ml.p3.8xlarge'
num_gpus=4

# the original max batch size that can fit to GPU memory without compiler
batch_size_native=16
learning_rate_native=float('5e-5')

# an updated max batch size that can fit to GPU memory with compiler
batch_size=26

# update learning rate
learning_rate=learning_rate_native/batch_size_native*batch_size*num_gpus*instance_count

training_script="your_training_script.py"

hyperparameters={
    "n_gpus": num_gpus,
    "batch_size": batch_size,
    "learning_rate": learning_rate,
    "training_script": training_script     # Specify the file name of your training script.
}

pytorch_huggingface_estimator=HuggingFace(
    entry_point='distributed_training_launcher.py',    # Specify the distributed training launcher script.
    instance_count=instance_count,
    instance_type=instance_type,
    transformers_version='4.17.0',
    pytorch_version='1.10.2',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

pytorch_huggingface_estimator.fit()
```

The launcher script should look like the following\. It wraps your training script and configures the distributed training environment depending on the size of the training instance of your choice\. 

```
# distributed_training_launcher.py

#!/bin/python

import subprocess
import sys

if __name__ == "__main__":
    arguments_command = " ".join([arg for arg in sys.argv[1:]])
    """
    The following line takes care of setting up an inter-node communication
    as well as managing intra-node workers for each GPU.
    """
    subprocess.check_call("python -m torch_xla.distributed.sm_dist " + arguments_command, shell=True)
```

**Tip**  
To prepare your training script, see the following pages\.  
[For distributed training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-transformers-trainer-distributed) of a PyTorch model using Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)
[For distributed training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer-distributed) of a PyTorch model without Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)

**Tip**  
To find end\-to\-end examples, see the following notebooks:  
[Compile and Train the GPT2 Model using the Transformers Trainer API with the SST2 Dataset for Single\-Node Multi\-GPU Training](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_multiple_gpu_single_node/language-modeling-multi-gpu-single-node.html)
[Compile and Train the GPT2 Model using the Transformers Trainer API with the SST2 Dataset for Multi\-Node Multi\-GPU Training](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_multiple_gpu_multiple_node/language-modeling-multi-gpu-multi-node.html)

------

The following list is the minimal set of parameters required to run a SageMaker training job with the compiler\.

**Note**  
When using the SageMaker Hugging Face estimator, you must specify the `transformers_version`, `pytorch_version`, `hyperparameters`, and `compiler_config` parameters to enable SageMaker Training Compiler\. You cannot use `image_uri` to manually specify the Training Compiler integrated Deep Learning Containers that are listed at [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.
+ `entry_point` \(str\) – Required\. Specify the file name of your training script\.
**Note**  
To run a distributed training with SageMaker Training Compiler and PyTorch v1\.10\.2 and before, specify the file name of a launcher script to this parameter\. The launcher script should be prepared to wrap your training script and configure the distributed training environment\. For more information, see the following example notebooks:  
[Compile and Train the GPT2 Model using the Transformers Trainer API with the SST2 Dataset for Single\-Node Multi\-GPU Training](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_multiple_gpu_single_node/language-modeling-multi-gpu-single-node.html)
[Compile and Train the GPT2 Model using the Transformers Trainer API with the SST2 Dataset for Multi\-Node Multi\-GPU Training](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_multiple_gpu_multiple_node/language-modeling-multi-gpu-multi-node.html)
+ `source_dir` \(str\) – Optional\. Add this if need to install additional packages\. To install packages, you need to prapare a `requirements.txt` file under this directory\.
+ `instance_count` \(int\) – Required\. Specify the number of instances\.
+ `instance_type` \(str\) – Required\. Specify the instance type\.
+ `transformers_version` \(str\) – Required only when using the SageMaker Hugging Face estimator\. Specify the Hugging Face Transformers library version supported by SageMaker Training Compiler\. To find available versions, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.
+ `framework_version` or `pytorch_version` \(str\) – Required\. Specify the PyTorch version supported by SageMaker Training Compiler\. To find available versions, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.
**Note**  
When using the SageMaker Hugging Face estimator, you must specify both `transformers_version` and `pytorch_version`\.
+ `hyperparameters` \(dict\) – Optional\. Specify hyperparameters for the training job, such as `n_gpus`, `batch_size`, and `learning_rate`\. When you enable SageMaker Training Compiler, try larger batch sizes and adjust the learning rate accordingly\. To find case studies of using the compiler and adjusted batch sizes to improve training speed, see [Tested Models](training-compiler-support.md#training-compiler-tested-models) and [SageMaker Training Compiler Example Notebooks and Blogs](training-compiler-examples-and-blogs.md)\.
**Note**  
To run a distributed training with SageMaker Training Compiler and PyTorch v1\.10\.2 and before, you need to add an additional parameter, `"training_script"`, to specify your training script, as shown in the preceding code example\.
+ `compiler_config` \(TrainingCompilerConfig object\) – Required to activate SageMaker Training Compiler\. Include this parameter to turn on SageMaker Training Compiler\. The following are parameters for the `TrainingCompilerConfig` class\.
  + `enabled` \(bool\) – Optional\. Specify `True` or `False` to turn on or turn off SageMaker Training Compiler\. The default value is `True`\.
  + `debug` \(bool\) – Optional\. To receive more detailed training logs from your compiler\-accelerated training jobs, change it to `True`\. However, the additional logging might add overhead and slow down the compiled training job\. The default value is `False`\.
+ `distribution` \(dict\) – Optional\. To run a distributed training job with SageMaker Training Compiler, add `distribution = { 'pytorchxla' : { 'enabled': True }}`\.

**Warning**  
If you turn on SageMaker Debugger, it might impact the performance of SageMaker Training Compiler\. We recommend that you turn off Debugger when running SageMaker Training Compiler to make sure there's no impact on performance\. For more information, see [Considerations](training-compiler-tips-pitfalls.md#training-compiler-tips-pitfalls-considerations)\. To turn the Debugger functionalities off, add the following two arguments to the estimator:  

```
disable_profiler=True,
debugger_hook_config=False
```

If the training job with the compiler is launched successfully, you receive the following logs during the job initialization phase: 
+ With `TrainingCompilerConfig(debug=False)`

  ```
  Found configuration for Training Compiler
  Configuring SM Training Compiler...
  ```
+ With `TrainingCompilerConfig(debug=True)`

  ```
  Found configuration for Training Compiler
  Configuring SM Training Compiler...
  Training Compiler set to debug mode
  ```

## Using the SageMaker `CreateTrainingJob` API Operation<a name="training-compiler-enable-pytorch-api"></a>

SageMaker Training Compiler configuration options must be specified through the `AlgorithmSpecification` and `HyperParameters` field in the request syntax for the [`CreateTrainingJob` API operation](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\.

```
"AlgorithmSpecification": {
    "TrainingImage": "<sagemaker-training-compiler-enabled-dlc-image>"
},

"HyperParameters": {
    "sagemaker_training_compiler_enabled": "true",
    "sagemaker_training_compiler_debug_mode": "false",
    "sagemaker_pytorch_xla_multi_worker_enabled": "false"    // set to "true" for distributed training
}
```

To find a complete list of deep learning container image URIs that have SageMaker Training Compiler implemented, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.