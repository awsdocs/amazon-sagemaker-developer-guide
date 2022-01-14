# Step 2: Launch a Training Job Using the SageMaker Python SDK<a name="model-parallel-sm-sdk"></a>

The SageMaker Python SDK supports managed training of TensorFlow and PyTorch models\. To launch a training job using one of these frameworks, you can define a [TensorFlow `Estimator`](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) or a [PyTorch `Estimator`](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html#pytorch-estimator)\. 

The TensorFlow and PyTorch `Estimator` object contains a `distribution` parameter, which is used to enable and specify parameters for the initialization of SageMaker's distributed model parallel library\. The library internally uses MPI, so in order to use model parallelism, MPI must be enabled using the `distribution` parameter\. 

The following is an example of how you can launch a PyTorch training job with the library\.

```
import sagemaker
sagemaker_session = sagemaker.session.Session(boto_session=session)

smp_options = {
    "enabled":True,
    "parameters": {
        "partitions": 2,
        "microbatches": 4,
        "placement_strategy": "spread",
        "pipeline": "interleaved",
        "optimize": "speed",
        "ddp": True,
    }
}

mpi_options = {
    "enabled" : True,
    "processes_per_host" : 8,
    # "custom_mpi_options" : "--mca btl_vader_single_copy_mechanism none"
}

smd_mp_estimator = PyTorch(
    entry_point="your_training_script.py", # Specify your train script
    source_dir="location_to_your_script",
    role=role,
    instance_type='ml.p3.16xlarge',
    sagemaker_session=sagemaker_session,
    framework_version='1.8.1',
    py_version='py36',
    instance_count=1,
    distribution={
        "smdistributed": {"modelparallel": smp_options},
        "mpi": mpi_options
    },
    base_job_name="SMD-MP-demo",
)

smd_mp_estimator.fit('s3://my_bucket/my_training_data/')
```

To enable the library, you need to pass dictionaries for the `"mpi"` and `"smdistributed"` keys to the `distribution` argument of the estimator constructors in SageMaker Python SDK\. 
+ For the `"smdistributed"` key, pass a dictionary with the `"modelparallel"` key and the following inner dictionaries\. 
**Note**  
Using `"modelparallel"` and `"dataparallel"` in one training job is not supported\. 
  + `"enabled"` \(required\) – To enable model parallelism, set `"enabled": True`\.
  + `"parameters"` \(required\) – Specify a set of parameters for model parallelism listed in [`smdistributed` Parameters](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html#smdistributed-parameters)\. Among the parameters, the `"partitions"` key is required to specify how many model partitions need to be requested for a training job\. 
+ For the `"mpi"` key, pass a dictionary that contains the following:
  + `"enabled"` \(required\) – `True` to launch the training job with MPI\.
  + `"processes_per_host"` \(required\) – Specify the number of processes MPI should launch on each host\. In SageMaker a host is a single [Amazon EC2 ml instance]()\. The SageMaker Python SDK maintains a one\-to\-one mapping between processes and GPUs across model and data parallelism\. This means that SageMaker schedules each process on a single, separate GPU and no GPU contains more than one process\. If you are using PyTorch, you must restrict each process to its own device through `torch.cuda.set_device(smp.local_rank())`\. To learn more, see [PyTorch](model-parallel-customize-training-script-pt.md#model-parallel-customize-training-script-pt-16)\.
**Important**  
 `process_per_host` *must* not be greater than the number of GPUs per instance and typically will be equal to the number of GPUs per instance\.

    For example, if you use one instance with 4\-way model parallelism and 2\-way data parallelism, then `processes_per_host` should be 2 x 4 = 8\. Therefore, you must choose an instance that has at least 8 GPUs, such as an ml\.p3\.16xlarge\.

    The following image illustrates how 2\-way data parallelism and 4\-way model parallelism is distributed across 8 GPUs: the models is partitioned across 4 GPUs, and each partition is added to 2 GPUs\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/model-data-parallel.png)
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

To launch the training job using the estimator and your SageMaker model parallel configured training script, run the `estimator.fit()` function \(`smd_mp_estimator.fit()` in the previous code example\)\. You can launch a SageMaker training job using a SageMaker notebook instance, or locally\. 
+ Using a SageMaker notebook instance is recommended for new users\. To see an example of how you can launch a training job using a SageMaker notebook instance, see [Amazon SageMaker Distributed Training Notebook Examples](distributed-training-notebook-examples.md)\.
+ You can launch locally if you have [set up your AWS credentials and Region for development](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)\.

Use the following resources to learn more about using the SageMaker Python SDK with these frameworks:
+ [Use TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html)
+ [Use PyTorch with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html)

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