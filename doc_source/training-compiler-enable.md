# Enable SageMaker Training Compiler<a name="training-compiler-enable"></a>

SageMaker Training Compiler is built into the SageMaker Python SDK and AWS Deep Learning Containers so that you don’t need to change your workflows to enable Training Compiler\. You can use any of the SageMaker interfaces: Amazon SageMaker Studio, Amazon SageMaker notebook instances, AWS SDK for Python \(Boto3\), and AWS Command Line Interface, to run training jobs in the same way as you already do\.

**Topics**
+ [Enable SageMaker Training Compiler Using the SageMaker Python SDK](#training-compiler-enable-pysdk)
+ [Enable SageMaker Training Compiler Using the SageMaker `CreateTrainingJob` API Operation](#training-compiler-enable-api)

## Enable SageMaker Training Compiler Using the SageMaker Python SDK<a name="training-compiler-enable-pysdk"></a>

To enable SageMaker Training Compiler, add the `compiler_config` parameter to the `HuggingFace` estimator\. Import the `TrainingCompilerConfig` class and pass an instance to the parameter\. The following code shows the basic structure of a SageMaker estimator class with SageMaker Training Compiler enabled\.

**Tip**  
To get started, try using the batch sizes provided in the reference table at [Tested Models](training-compiler-support.md#training-compiler-tested-models)\.

**Note**  
SageMaker Training Compiler is currently available only through the SageMaker `HuggingFace` estimator\.

For information that fits your use case, see one of the following options\.

### For single GPU training<a name="training-compiler-estimator-single"></a>

------
#### [ Hugging Face Estimator for TensorFlow ]

```
from sagemaker.huggingface import HuggingFace, TrainingCompilerConfig

# the original max batch size that can fit into GPU memory without compiler
batch_size_native=12
learning_rate_native=float('5e-5')

# an updated max batch size that can fit into GPU memory with compiler
batch_size=64

# update the global learning rate
learning_rate=learning_rate_native/batch_size_native*batch_size

hyperparameters={
    "n_gpus": 1,
    "batch_size": batch_size,
    "learning_rate": learning_rate
}

tensorflow_huggingface_estimator=HuggingFace(
    entry_point='train.py',
    instance_count=1,
    instance_type='ml.p3.2xlarge',
    transformers_version='4.11.0',
    tensorflow_version='2.5.1',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

tensorflow_huggingface_estimator.fit()
```

After you finish setting up the estimator, see the following pages to prepare your training script:
+ [For single GPU training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-transformers-keras-single-gpu) of a TensorFlow Keras model with Hugging Face Transformers
+ [For single GPU training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-transformers-no-keras-single-gpu) of a TensorFlow model with Hugging Face Transformers

------
#### [ Hugging Face Estimator for PyTorch ]

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
    transformers_version='4.11.0',
    pytorch_version='1.9.0',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

pytorch_huggingface_estimator.fit()
```

After you finish setting up the estimator, see the following pages to prepare your training script:
+ [For single GPU training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-transformers-trainer-single-gpu) of a PyTorch model using Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)
+ [For single GPU training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer-single-gpu) of a PyTorch model without Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)

To find end\-to\-end examples, see the following notebooks:
+ [Compile and Train a Hugging Face Transformers Trainer Model for Question and Answering with the SQuAD dataset ](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_single_gpu_single_node/albert-base-v2/albert-base-v2.html) 
+ [Compile and Train a Hugging Face Transformer `BERT` Model with the SST Dataset using SageMaker Training Compiler](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_single_gpu_single_node/bert-base-cased/bert-base-cased-single-node-single-gpu.html) 
+ [Compile and Train a Binary Classification Trainer Model with the SST2 Dataset for Single\-Node Single\-GPU Training ](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_single_gpu_single_node/roberta-base/roberta-base.html)

------

### For distributed training<a name="training-compiler-estimator-distributed"></a>

------
#### [ Hugging Face Estimator for TensorFlow ]

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

tensorflow_huggingface_estimator=HuggingFace(
    entry_point='train.py',
    instance_count=instance_count,
    instance_type=instance_type,
    transformers_version='4.11.0',
    tensorflow_version='2.5.1',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

tensorflow_huggingface_estimator.fit()
```

**Tip**  
After you finish setting up the estimator, see the following pages to prepare your training script:  
[For distributed training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-transformers-keras-distributed) of a TensorFlow Keras model with Hugging Face Transformers
[For distributed training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-transformers-no-keras-distributed) of a TensorFlow model with Hugging Face Transformers

------
#### [ Hugging Face Estimator for PyTorch ]

For PyTorch, SageMaker Training Compiler uses an alternate mechanism for distributed training, and you don’t need to specify the `distribution` parameter\. The compiler requires you to pass a SageMaker distributed training launcher script to the entry point and pass your training script to the `hyperparameters` argument\.

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
    "training_script": training_script
}

pytorch_huggingface_estimator=HuggingFace(
    entry_point='distributed_training_launcher.py',
    instance_count=instance_count,
    instance_type=instance_type,
    transformers_version='4.11.0',
    pytorch_version='1.9.0',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

pytorch_huggingface_estimator.fit()
```

The launcher script wraps your training script and sets up the distributed training environment depending on the size of the training instance of your choice\. The launcher script should look like the following:

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

After you finish setting up the estimator, see the following pages to prepare your training script:
+ [For distributed training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-transformers-trainer-distributed) of a PyTorch model using Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)
+ [For distributed training](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer-distributed) of a PyTorch model without Hugging Face Transformers' [Trainer API](https://huggingface.co/transformers/main_classes/trainer.html)

**Tip**  
To find end\-to\-end examples, see the following notebooks:  
[Compile and Train the GPT2 Model using the Transformers Trainer API with the SST2 Dataset for Single\-Node Multi\-GPU Training](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_multiple_gpu_single_node/language-modeling-multi-gpu-single-node.html)
[Compile and Train the GPT2 Model using the Transformers Trainer API with the SST2 Dataset for Multi\-Node Multi\-GPU Training](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_multiple_gpu_multiple_node/language-modeling-multi-gpu-multi-node.html)

------

**Important**  
You must specify the `transformers_version`, `pytorch_version` or `tensorflow_version`, `hyperparameters`, and `compiler_config` parameters to enable SageMaker Training Compiler\. You cannot use `image_uri` to choose the Training Compiler integrated Deep Learning Containers that are listed at [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.

The following list is the minimal set of parameters required to run a SageMaker training job with the compiler\.
+ `entry_point` \(str\) – Required\. Specify the filename of your training script\.
**Note**  
To run a PyTorch distributed training job with SageMaker Training Compiler, specify the filename of a launcher script that wraps your training script and configures the distributed training job properly\. For more information, see the following notebooks:  
[Compile and Train the GPT2 Model using the Transformers Trainer API with the SST2 Dataset for Single\-Node Multi\-GPU Training](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_multiple_gpu_single_node/language-modeling-multi-gpu-single-node.html)
[Compile and Train the GPT2 Model using the Transformers Trainer API with the SST2 Dataset for Multi\-Node Multi\-GPU Training](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-training-compiler/huggingface/pytorch_multiple_gpu_multiple_node/language-modeling-multi-gpu-multi-node.html)
+ `instance_count` \(int\) – Required\. Specify the number of instances\.
+ `instance_type` \(str\) – Required\. Specify the instance type\.
+ `transformers_version` \(str\) – Required\. Specify the Hugging Face Transformers library version supported by SageMaker Training Compiler\. To find available versions, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.
+ `pytorch_version` or `tensorflow_version` \(str\) – Required\. Specify the PyTorch or TensorFlow version supported by SageMaker Training Compiler\. To find available versions, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.
+ `hyperparameters` \(dict\) – Optional\. Specify hyperparameters for the training job, such as `n_gpus`, `batch_size`, and `learning_rate`\. When you enable SageMaker Training Compiler, try larger batch sizes and adjust the learning rate accordingly\. To find examples, see [SageMaker Training Compiler Example Notebooks and Blogs](training-compiler-examples-and-blogs.md)\.
**Note**  
To run a PyTorch distributed training job with SageMaker Training Compiler, you need to add an additional parameter, `"training_script"`, to specify your training script\. For example:  

  ```
  "training_script": "<your_dl_training_script.py>"
  ```
+ `compiler_config` \(TrainingCompilerConfig object\) – Required\. Include this parameter to enable SageMaker Training Compiler\. The following are parameters for the `TrainingCompilerConfig` class\.
  + `enabled` \(bool\) – Optional\. Specify `True` or `False` to enable or disable SageMaker Training Compiler\. The default value is `True`\.
  + `debug` \(bool\) – Optional\. To receive more detailed training logs from your compiler\-accelerated training jobs, change it to `True`\. However, the additional logging might add overhead and slow down the compiled training job\. The default value is `False`\.

**Warning**  
If you turn on SageMaker Debugger, it might impact the performance of SageMaker Training Compiler\. We recommend that you turn off Debugger when running SageMaker Training Compiler to make sure there's no impact on performance\. To turn the Debugger functionalities off, add the following two arguments to the estimator:  

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

## Enable SageMaker Training Compiler Using the SageMaker `CreateTrainingJob` API Operation<a name="training-compiler-enable-api"></a>

SageMaker Training Compiler configuration options must be specified through the `AlgorithmSpecification` and `HyperParameters` field in the request syntax for the [`CreateTrainingJob` API operation](http://amazonaws.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\.

```
"AlgorithmSpecification": {
    "TrainingImage": "<sagemaker-training-compiler-enabled-dlc-image>"
},

"HyperParameters": {
    "sagemaker_training_compiler_enabled": "true",
    "sagemaker_training_compiler_debug_mode": "false"
}
```

To find a complete list of deep learning container image URIs that have SageMaker Training Compiler implemented, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.