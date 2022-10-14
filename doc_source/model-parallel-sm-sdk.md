# Step 2: Launch a Training Job Using the SageMaker Python SDK<a name="model-parallel-sm-sdk"></a>

The SageMaker Python SDK supports managed training of models with ML frameworks such as TensorFlow and PyTorch\. To launch a training job using one of these frameworks, you define a SageMaker [TensorFlow estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator), a SageMaker [PyTorch estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html#pytorch-estimator), or a SageMaker generic [Estimator](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator) to use the modified training script and model parallelism configuration\.

**Topics**
+ [Using the SageMaker TensorFlow and PyTorch Estimators](#model-parallel-using-sagemaker-pysdk)
+ [Extend a Prebuilt Docker Container that Contains SageMaker's Distributed Model Parallel Library](#model-parallel-customize-container)
+ [Create Your Own Docker Container with the SageMaker Distributed Model Parallel Library](#model-parallel-bring-your-own-container)

## Using the SageMaker TensorFlow and PyTorch Estimators<a name="model-parallel-using-sagemaker-pysdk"></a>

The TensorFlow and PyTorch estimator classes contain the `distribution` parameter, which you can use to specify configuration parameters for using distributed training frameworks\. The SageMaker model parallel library internally uses MPI for hybrid data and model parallelism, so you must use the MPI option with the library\.

The following template of a TensorFlow or PyTorch estimator shows how to configure the `distribution` parameter for using the SageMaker model parallel library with MPI\.

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
    framework_version='2.6.3',
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
    role=sagemaker.get_execution_role(),
    instance_count=1,
    instance_type='ml.p3.16xlarge',
    framework_version='1.12.0',
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

**Configuration parameters for SageMaker model parallelism**
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

Use the following resources to learn more about using the model parallelism features in the SageMaker Python SDK:
+ [Use TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html)
+ [Use PyTorch with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html)
+ We recommend you use a SageMaker notebook instance if you are new users\. To see an example of how you can launch a training job using a SageMaker notebook instance, see [Amazon SageMaker Distributed Training Notebook Examples](distributed-training-notebook-examples.md)\.
+ You can also submit a distributed training job from your machine using AWS CLI\. To set up AWS CLI on your machine, see [set up your AWS credentials and Region for development](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)\.

## Extend a Prebuilt Docker Container that Contains SageMaker's Distributed Model Parallel Library<a name="model-parallel-customize-container"></a>

To extend a prebuilt container and use SageMaker's distributed model parallel library, you must use one of the available AWS Deep Learning Containers \(DLC\) images for PyTorch or TensorFlow\. The SageMaker distributed model parallel library is included in the TensorFlow \(2\.3\.0 and later\) and PyTorch \(1\.6\.0 and later\) DLC images with CUDA \(`cuxyz`\)\. For a complete list of DLC images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) in the *AWS Deep Learning Containers GitHub repository*\.

**Tip**  
We recommend that you use the image that contains the latest version of TensorFlow or PyTorch to access the most up\-to\-date version of the SageMaker distributed model parallel library\.

For example, your Dockerfile should contain a `FROM` statement similar to the following:

```
# Use the SageMaker DLC image URI for TensorFlow or PyTorch
FROM aws-dlc-account-id.dkr.ecr.aws-region.amazonaws.com/framework-training:{framework-version-tag}

# Add your dependencies here
RUN ...

ENV PATH="/opt/ml/code:${PATH}"

# this environment variable is used by the SageMaker container to determine our user code directory.
ENV SAGEMAKER_SUBMIT_DIRECTORY /opt/ml/code
```

Additionally, when you define a PyTorch or TensorFlow estimator, you must specify that the `entry_point` for your training script\. This should be the same path identified with `ENV SAGEMAKER_SUBMIT_DIRECTORY` in your Dockerfile\. 

**Tip**  
You must push this Docker container to Amazon Elastic Container Registry \(Amazon ECR\) and use the image URI \(`image_uri`\) to define a SageMaker estimator for training\. For more information, see [Extend a Prebuilt Container](https://docs.aws.amazon.com/sagemaker/latest/dg/prebuilt-containers-extend.html)\. 

After you finish hosting the Docker container and retrieving the image URI of the container, create a SageMaker `PyTorch` estimator object as follows\. This example assumes that you have already defined `smp_options` and `mpi_options`\. 

```
smd_mp_estimator = Estimator(
    entry_point="your_training_script.py",
    role=sagemaker.get_execution_role(),
    instance_type='ml.p3.16xlarge',
    sagemaker_session=sagemaker_session,
    image_uri='your_aws_account_id.dkr.ecr.region.amazonaws.com/name:tag'
    instance_count=1,
    distribution={
        "smdistributed": smp_options,
        "mpi": mpi_options
    },
    base_job_name="SMD-MP-demo",
)

smd_mp_estimator.fit('s3://my_bucket/my_training_data/')
```

## Create Your Own Docker Container with the SageMaker Distributed Model Parallel Library<a name="model-parallel-bring-your-own-container"></a>

To build your own Docker container for training and use the SageMaker model parallel library, you must include the correct dependencies and the binary files of the SageMaker distributed parallel libraries in your Dockerfile\. This section provides the minimum set of code blocks you must include to properly prepare a SageMaker training environment and the model parallel library in your own Docker container\.

**Note**  
This custom Docker option with the SageMaker model parallel library as a binary is available only for PyTorch\.

**To create a Dockerfile with the SageMaker training toolkit and the model parallel library**

1. Start with one of the [NVIDIA CUDA base images](https://hub.docker.com/r/nvidia/cuda)\.

   ```
   FROM <cuda-cudnn-base-image>
   ```
**Tip**  
The official AWS Deep Learning Container \(DLC\) images are built from the [NVIDIA CUDA base images](https://hub.docker.com/r/nvidia/cuda)\. We recommend you look into the [official Dockerfiles of AWS Deep Learning Container for PyTorch](https://github.com/aws/deep-learning-containers/tree/master/pytorch/training/docker) to find which versions of the libraries you need to install and how to configure them\. The official Dockerfiles are complete, benchmark tested, and managed by the SageMaker and Deep Learning Container service teams\. In the provided link, choose the PyTorch version you use, choose the CUDA \(`cuxyz`\) folder, and choose the Dockerfile ending with `.gpu` or `.sagemaker.gpu`\.

1. To set up a distributed training environment, you need to install software for communication and network devices, such as [Elastic Fabric Adapter \(EFA\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/efa.html), [NVIDIA Collective Communications Library \(NCCL\)](https://developer.nvidia.com/nccl), and [Open MPI](https://www.open-mpi.org/)\. Depending on the PyTorch and CUDA versions you choose, you must install compatible versions of the libraries\.
**Important**  
Because the SageMaker model parallel library requires the SageMaker data parallel library in the subsequent steps, we highly recommend that you follow the instructions at [Create Your Own Docker Container with the SageMaker Distributed Data Parallel Library](data-parallel-use-api.md#data-parallel-bring-your-own-container) to properly set up a SageMaker training environment for distributed training\.

   For more information about setting up EFA with NCCL and Open MPI, see [Get started with EFA and MPI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/efa-start.html) and [Get started with EFA and NCCL](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/efa-start-nccl.html)\.

1. Add the following arguments to specify the URLs of the SageMaker distributed training packages for PyTorch\. The SageMaker model parallel library requires the SageMaker data parallel library to use the cross\-node Remote Direct Memory Access \(RDMA\)\.

   ```
   ARG SMD_MODEL_PARALLEL_URL=https://sagemaker-distributed-model-parallel.s3.us-west-2.amazonaws.com/pytorch-1.10.0/build-artifacts/2022-02-21-19-26/smdistributed_modelparallel-1.7.0-cp38-cp38-linux_x86_64.whl
   ARG SMDATAPARALLEL_BINARY=https://smdataparallel.s3.amazonaws.com/binary/pytorch/1.10.2/cu113/2022-02-18/smdistributed_dataparallel-1.4.0-cp38-cp38-linux_x86_64.whl
   ```

1. Install dependencies that the SageMaker model parallel library requires\.

   1. Install the [METIS](http://glaros.dtc.umn.edu/gkhome/metis/metis/overview) library\.

      ```
      ARG METIS=metis-5.1.0
      
      RUN rm /etc/apt/sources.list.d/* \
        && wget -nv http://glaros.dtc.umn.edu/gkhome/fetch/sw/metis/${METIS}.tar.gz \
        && gunzip -f ${METIS}.tar.gz \
        && tar -xvf ${METIS}.tar \
        && cd ${METIS} \
        && apt-get update \
        && make config shared=1 \
        && make install \
        && cd .. \
        && rm -rf ${METIS}.tar* \
        && rm -rf ${METIS} \
        && rm -rf /var/lib/apt/lists/* \
        && apt-get clean
      ```

   1. Install the [RAPIDS Memory Manager library](https://github.com/rapidsai/rmm#rmm-rapids-memory-manager)\. This requires [CMake](https://cmake.org/) 3\.14 or later\.

      ```
      ARG RMM_VERSION=0.15.0
      
      RUN  wget -nv https://github.com/rapidsai/rmm/archive/v${RMM_VERSION}.tar.gz \
        && tar -xvf v${RMM_VERSION}.tar.gz \
        && cd rmm-${RMM_VERSION} \
        && INSTALL_PREFIX=/usr/local ./build.sh librmm \
        && cd .. \
        && rm -rf v${RMM_VERSION}.tar* \
        && rm -rf rmm-${RMM_VERSION}
      ```

1. Install the SageMaker model parallel library\.

   ```
   RUN pip install --no-cache-dir -U ${SMD_MODEL_PARALLEL_URL}
   ```

1. Install the SageMaker data parallel library\.

   ```
   RUN SMDATAPARALLEL_PT=1 pip install --no-cache-dir ${SMDATAPARALLEL_BINARY}
   ```

1. Install the [sagemaker\-training toolkit](https://github.com/aws/sagemaker-training-toolkit)\. The toolkit contains the common functionality that's necessary to create a container compatible with the SageMaker training platform and the SageMaker Python SDK\.

   ```
   RUN pip install sagemaker-training
   ```

1. After you finish creating the Dockerfile, see [Adapting Your Own Training Container](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html) to learn how to build the Docker container and host it in Amazon ECR\.

**Tip**  
For more general information about creating a custom Dockerfile for training in SageMaker, see [Use Your Own Training Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo.html)\.