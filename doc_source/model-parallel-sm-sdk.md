# Step 2: Launch a Training Job Using the SageMaker Python SDK<a name="model-parallel-sm-sdk"></a>

The SageMaker Python SDK supports managed training of models with ML frameworks such as TensorFlow and PyTorch\. To launch a training job using one of these frameworks, you define a [TensorFlow `Estimator`](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) or a [PyTorch `Estimator`](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html#pytorch-estimator) to use the modified training script and model parallelism configuration\.

The TensorFlow and PyTorch `Estimator` object contains a `distribution` parameter, which you need to use to specify configuration parameters for SageMaker's distributed model parallel library\. The library internally uses MPI for hybrid data and model parallelism, so you must use the MPI option with the library\.

The following is an example of how you can launch a TensorFlow or PyTorch distributed training job with the SageMaker model parallelism configuration\.

------
#### [ Using the SageMaker TensorFlow estimator ]

```
import sagemaker
from sagemaker.tensorflow import TensorFlow

smp_options = {
    "enabled":True,              # Required
    "parameters": {
        "partitions": 2,         # Required
        "microbatches": 4,
        "placement_strategy": "spread",
        "pipeline": "interleaved",
        "optimize": "speed",
        "horovod": True,         # Use this for hybrid model and data parallelism
    }
}

mpi_options = {
    "enabled" : True,            # Required
    "processes_per_host" : 8,    # Required
    # "custom_mpi_options" : "--mca btl_vader_single_copy_mechanism none"
}

smd_mp_estimator = TensorFlow(
    entry_point="your_training_script.py", # Specify your train script
    source_dir="location_to_your_script",
    role=sagemaker.get_execution_role(),
    instance_count=1,
    instance_type='ml.p3.16xlarge',
    framework_version='2.6.0',
    py_version='py38',
    distribution={
        "smdistributed": {"modelparallel": smp_options},
        "mpi": mpi_options
    },
    base_job_name="SMD-MP-demo",
)

smd_mp_estimator.fit('s3://my_bucket/my_training_data/')
```

------
#### [ Using the SageMaker PyTorch estimator ]

```
import sagemaker
from sagemaker.pytorch import PyTorch

smp_options = {
    "enabled":True,
    "parameters": {                        # Required
        "pipeline_parallel_degree": 2,     # Required
        "microbatches": 4,
        "placement_strategy": "spread",
        "pipeline": "interleaved",
        "optimize": "speed",
        "ddp": True,
    }
}

mpi_options = {
    "enabled" : True,                      # Required
    "processes_per_host" : 8,              # Required
    # "custom_mpi_options" : "--mca btl_vader_single_copy_mechanism none"
}

smd_mp_estimator = PyTorch(
    entry_point="your_training_script.py", # Specify your train script
    source_dir="location_to_your_script",
    role=role,
    instance_count=1,
    instance_type='ml.p3.16xlarge',
    framework_version='1.10.2',
    py_version='py38',
    distribution={
        "smdistributed": {"modelparallel": smp_options},
        "mpi": mpi_options
    },
    base_job_name="SMD-MP-demo",
)

smd_mp_estimator.fit('s3://my_bucket/my_training_data/')
```

------

To enable the library, you need to pass configuration dictionaries to the `"smdistributed"` and `"mpi"` keys through the `distribution` argument of the SageMaker estimator constructors\.

**Configuration parameters required for SageMaker model parallelism**
+ For the `"smdistributed"` key, pass a dictionary with the `"modelparallel"` key and the following inner dictionaries\. 
**Note**  
Using `"modelparallel"` and `"dataparallel"` in one training job is not supported\. 
  + `"enabled"` – Required\. To enable model parallelism, set `"enabled": True`\.
  + `"parameters"` – Required\. Specify a set of parameters for SageMaker model parallelism\.
    + For a complete list of common parameters, see [Parameters for `smdistributed`](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html#smdistributed-parameters) in the *SageMaker Python SDK documentation*\.

      For TensorFlow, see [TensorFlow\-specific Parameters](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html#tensorflow-specific-parameters)\.

      For PyTorch, see [PyTorch\-specific Parameters](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html#pytorch-specific-parameters)\.
    + `"pipeline_parallel_degree"` \(or `"partitions"` in `smdistributed-modelparallel<v1.6.0`\) – Required\. Among the [parameters for `smdistributed`](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html#smdistributed-parameters), this parameter is required to specify how many model partitions you want to split into\.
**Important**  
There is a breaking change in the parameter name\. The `"pipeline_parallel_degree"` parameter replaces the `"partitions"` since `smdistributed-modelparallel` v1\.6\.0\. For more information, see [Common Parameters](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html#common-parameters) for SageMaker model parallelism configuration and [SageMaker Distributed Model Parallel Release Notes](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_release_notes/smd_model_parallel_change_log.html) in the *SageMaker Python SDK documentation*\.
+ For the `"mpi"` key, pass a dictionary that contains the following:
  + `"enabled"` – Required\. Set `True` to launch the distributed training job with MPI\.
  + `"processes_per_host"` – Required\. Specify the number of processes MPI should launch on each host\. In SageMaker a host is a single [Amazon EC2 ML instance]()\. The SageMaker Python SDK maintains a one\-to\-one mapping between processes and GPUs across model and data parallelism\. This means that SageMaker schedules each process on a single, separate GPU and no GPU contains more than one process\. If you are using PyTorch, you must restrict each process to its own device through `torch.cuda.set_device(smp.local_rank())`\. To learn more, see [PyTorch](model-parallel-customize-training-script-pt.md#model-parallel-customize-training-script-pt-16)\.
**Important**  
 `process_per_host` *must* not be greater than the number of GPUs per instance and typically will be equal to the number of GPUs per instance\.
  + `"custom_mpi_options"` \(optional\) – Use this key to pass any custom MPI options you might need\. If you do not pass any MPI custom options to the key, the MPI option is set by default to the following flag\.

    ```
    --mca btl_vader_single_copy_mechanism none
    ```
**Note**  
You do not need to explicitly specify this default flag to the key\. If you explicitly specify it, your distributed model parallel training job might fail with the following error:  

    ```
    The following MCA parameter has been listed multiple times on the command line: 
    MCA param: btl_vader_single_copy_mechanism MCA parameters can only be listed once 
    on a command line to ensure there is no ambiguity as to its value. 
    Please correct the situation and try again.
    ```
**Tip**  
If you launch a training job using an EFA\-enabled instance type, such as `ml.p4d.24xlarge` and `ml.p3dn.24xlarge`, use the following flag for best performance:  

    ```
    -x FI_EFA_USE_DEVICE_RDMA=1 -x FI_PROVIDER=efa -x RDMAV_FORK_SAFE=1
    ```

To launch the training job using the estimator and your SageMaker model parallel configured training script, run the `estimator.fit()` function\.

## How the Library Splits a Model Given the Configuration Parameters<a name="model-parallel-how-it-splits-models"></a>

When you specify a value for the number of model partitions \(`pipeline_parallel_degree` for PyTorch or `partition` for TensorFlow\), the total number of GPUs \(`processes_per_host`\) must be divisible by the number of the model partitions\. To set this up properly, you have to specify the right values to the `pipeline_parallel_degree` and `processes_per_host` parameters\. The simple math is as follows:

```
(pipeline_parallel_degree) x (data_parallelism_degree) = processes_per_host
```

The library takes care of calculating the number of model replicas \(`data_parallelism_degree`\) given the two input parameters you provide\. For example, if you set `pipeline_parallel_degree=4` and `processes_per_host=8` to use an ML instance with eight GPU workers such as `ml.p3.16xlarge`, the library automatically sets up a two\-way data parallelism \(`data_parallelism_degree=2`\)\.

The following image illustrates how a two\-way data parallelism and four\-way model parallelism is distributed across eight GPUs: the model is partitioned across four GPUs, and each partition is assigned to two GPUs\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/model-data-parallel.png)

Use the following resources to learn more about using the model parallelism features in the SageMaker Python SDK:
+ [Use TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html)
+ [Use PyTorch with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html)
+ We recommend you use a SageMaker notebook instance if you are new users\. To see an example of how you can launch a training job using a SageMaker notebook instance, see [Amazon SageMaker Distributed Training Notebook Examples](distributed-training-notebook-examples.md)\.
+ You can also submit a distributed training job from your machine using AWS CLI\. To set up AWS CLI on your machine, see [set up your AWS credentials and Region for development](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)\.

## Extend a Docker Container that Contains SageMaker's Distributed Model Parallel Library<a name="model-parallel-customize-container"></a>

To extend a pre\-built container, or adapt your own container to use SageMaker's distributed model parallel library, you must use one of the PyTorch or TensorFlow GPU general framework base\-images\. The distributed model parallel library is included in all CUDA 11 \(`cu11x`\) TensorFlow 2\.3\.x and PyTorch 1\.6\.x versioned images and later\. See [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) for a list of available images\. 

**Tip**  
It is recommended that you use the image that contains the latest version of TensorFlow or PyTorch to access the most up to date version of the SageMaker distributed model parallel library\.

For example, if you are using PyTorch, your Dockerfile should contain a `FROM` statement similar to the following:

```
# SageMaker PyTorch image
FROM 763104351884.dkr.ecr.us-east-1.amazonaws.com/pytorch-training:{pytorch-version-tag}

# Add your dependencies here
RUN ...

ENV PATH="/opt/ml/code:${PATH}"

# this environment variable is used by the SageMaker PyTorch container to determine our user code directory.
ENV SAGEMAKER_SUBMIT_DIRECTORY /opt/ml/code

# /opt/ml and all subdirectories are utilized by SageMaker, use the /code subdirectory to store your user code.
COPY cifar10.py /opt/ml/code/cifar10.py
```

Additionally, when you define a PyTorch or TensorFlow `estimator`, you must specify that the `entry_point` for your training script\. This should be the same path identified with `ENV SAGEMAKER_SUBMIT_DIRECTORY` in your Dockerfile\. 

You must push this Docker container to Amazon Elastic Container Registry \(Amazon ECR\) and use the image URI \(`image_uri`\) to define an estimator\. See [Extend a Prebuilt Container](https://docs.aws.amazon.com/sagemaker/latest/dg/prebuilt-containers-extend.html) for more information\. 

For example, if your Dockerfile was defined using the preceding code block and used *name* and *tag* to push it to Amazon ECR, you would define a PyTorch `estimator` as follows\. This example assumes you have already defined `smp_options` and `mpi_options`\. 

```
smd_mp_estimator = PyTorch(
          entry_point='/opt/ml/code/pt_mnist.py', # Identify
          source_dir='utils',
          role=role,
          instance_type='ml.p3.16xlarge',
          sagemaker_session=sagemaker_session,
          image_uri='aws_account_id.dkr.ecr.region.amazonaws.com/name:tag'
          instance_count=1,
          distribution={
              "smdistributed": smp_options,
              "mpi": mpi_options
          },
          base_job_name="SMD-MP-demo",
      )

smd_mp_estimator.fit('s3://my_bucket/my_training_data/')
```