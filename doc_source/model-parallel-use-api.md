# Use the SageMaker Distributed Model Parallel API<a name="model-parallel-use-api"></a>

To use Amazon SageMaker's distributed model parallel library, you must create a training script for one of the supported frameworks and launch the training job using the SageMaker Python SDK\. To learn how you can incorporate the library into a training script, see [Modify Your Training Script Using SageMaker's Distributed Model Parallel Library](model-parallel-customize-training-script.md)\. The library's API documentation is located in the SageMaker Python SDK\. Refer to the [SageMaker's distributed model parallel API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\.

SageMaker supports the following training environment configurations\. 

1. You can use a prebuilt TensorFlow or PyTorch container\. This option is recommended for new users of the model parallel library, and is demonstrated in the example [MNIST with PyTorch 1\.6 and Amazon SageMaker's distributed model parallel library](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/model_parallel/mnist/pytorch_smmodelparallel_mnist.html)\.

1. You can extend SageMaker prebuilt containers to handle any additional functional requirements for your algorithm or model that the prebuilt SageMaker Docker image doesn't support\. To see an example of how you can extend a pre\-built container, see [Extend a Prebuilt Container](https://docs.aws.amazon.com/sagemaker/latest/dg/prebuilt-containers-extend.html)\.

1. You can adapt your own Docker container to work with SageMaker using the [SageMaker Training toolkit](https://github.com/aws/sagemaker-training-toolkit)\. For an example, see [Adapting Your Own Training Container](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html)\.

For options 2 and 3 in the preceding list, refer to [Extend or Adapt A Docker Container that Contains SageMaker's Distributed Model Parallel Library](#model-parallel-customize-container) to learn how to install the model parallel library in an extended or customized Docker container\. 

In all cases, you launch your job using a SageMaker Python SDK TensorFlow or PyTorch `Estimator` to initialize the library and launch a training job\. See the following section, [Launch a Training Job with the SageMaker Python SDK](#model-parallel-sm-sdk), to learn more\.

## Launch a Training Job with the SageMaker Python SDK<a name="model-parallel-sm-sdk"></a>

The SageMaker Python SDK supports managed training of TensorFlow and PyTorch models\. To launch a training job using one of these frameworks, you can define a [TensorFlow `Estimator`](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) or a [PyTorch `Estimator`](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html#pytorch-estimator)\.

The TensorFlow and PyTorch `Estimator` object contains a `distribution` parameter, which is used to enable and specify parameters for the initialization of SageMaker's distributed model parallel library\. The library internally uses MPI, so in order to use model parallelism, MPI must be enabled using the `distribution` parameter\. 

The following is an example of how you can launch a new PyTorch training job with the library\.

```
sagemaker_session = sagemaker.session.Session(boto_session=session)

mpi_options = {
                "enabled" : True,
                "processes_per_host" : 8,
                "custom_mpi_options" : "--mca btl_vader_single_copy_mechanism none "
               }

smp_options = {
                "enabled":True,
                "parameters": {
                    "microbatches": 4,
                    "placement_strategy": "spread",
                    "pipeline": "interleaved",
                    "optimize": "speed",
                    "partitions": 2,
                    "ddp": True,
                }
            }

smd_mp_estimator = PyTorch(
          entry_point="pt_mnist.py", # Pick your train script
          source_dir='utils',
          role=role,
          instance_type='ml.p3.16xlarge',
          sagemaker_session=sagemaker_session,
          framework_version='1.6.0',
          # You must set py_version to py36
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

To enable the library, a dictionary with the keys `"mpi"` and `"smdistributed"` needs to be passed as the `distribution` argument of the TensorFlow and PyTorch Estimator constructors in Python SDK\. For the `"mpi"` key, a dict must be passed which contains:
+ `"enabled"`: `True` to launch the training job with MPI\.
+ `"processes_per_host"`: Specify the number of processes MPI should launch on each host\. In SageMaker a host is a single [Amazon EC2 ml instance]()\. The SageMaker Python SDK maintains a one\-to\-one mapping between processes and GPUs across model and data parallelism\. This means that SageMaker schedules each process on a single, separate GPU and no GPU contains more than one process\. If you are using PyTorch, you must restrict each process to its own device through `torch.cuda.set_device(smp.local_rank())`\. To learn more, see [PyTorch](model-parallel-customize-training-script-pt.md#model-parallel-customize-training-script-pt-16)\.
**Important**  
 `process_per_host` *must* be less than the number of GPUs per instance and typically will be equal to the number of GPUs per instance\.

  For example, if you use one instance with 4\-way model parallelism and 2\-way data parallelism, then `processes_per_host` should be 2 x 4 = 8\. Therefore, you must choose an instance that has at least 8 GPUs, such as an ml\.p3\.16xlarge\.

  The following image illustrates how 2\-way data parallelism and 4\-way model parallelism is distributed across 8 GPUs: the models is partitioned across 4 GPUs, and each partition is added to 2 GPUs\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/model-data-parallel.png)
+ `"custom_mpi_options"`: Use this key to pass any custom MPI options you might need\. To avoid Docker warnings from contaminating your training logs, we recommend the following flag\.

  ```
  --mca btl_vader_single_copy_mechanism none
  ```

For the `"smdistributed"` key, a dictionary must be passed which has the only key `"modelparallel"`\. Using `"modelparallel"` and `"dataparallel"` in the same training job is not supported\. 

For the `"modelparallel"` dictionary, an inner dictionary must be passed, which enables the modelparallel library by setting `"enabled": True`, as well as a dict of `"parameters"` to customize the library\. Among the parameters, the `"partitions"` key is required, which specifies how many model partitions are requested\. To learn more about values you can use in `parameters`, see the [SageMaker distributed data parallel API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel.html)\.

To launch the training job using SageMaker model parallel configured training script, you use the `Estimator.fit()` function\. You can launch a SageMaker training job using a SageMaker notebook instance, or locally\. 
+ Using a SageMaker notebook instance is recommended for new users\. To see an example of how you can launch a training job using a SageMaker notebook instance, see [Distributed Training Jupyter Notebook Examples](distributed-training-notebook-examples.md)\.
+ You can launch locally if you have [ set up your AWS credentials and Region for development](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)\.

Use the following resources to learn more about using the SageMaker Python SDK with these frameworks:
+ [Use TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html)
+ [Use PyTorch with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html)

## Extend or Adapt A Docker Container that Contains SageMaker's Distributed Model Parallel Library<a name="model-parallel-customize-container"></a>

To extend a pre\-built container, or adapt your own container to use SageMaker's distributed model parallel library, you must use one of the following Pytorch or TensorFlow GPU general framework base\-images:
+ 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/tensorflow\-training:2\.4\.1\-gpu\-py37\-cu110\-ubuntu18\.04
+ 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/tensorflow\-training:2\.3\.1\-gpu\-py37\-cu110\-ubuntu18\.04
+ 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/pytorch\-training:1\.8\.0\-gpu\-py36\-cu111\-ubuntu18\.04
+ 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/pytorch\-training:1\.7\.1\-gpu\-py36\-cu110\-ubuntu18\.04
+ 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/pytorch\-training:1\.6\.0\-gpu\-py36\-cu110\-ubuntu18\.04 

For example, if you were using Pytorch 1\.7\.1 in us\-east\-1, your Dockerfile should contain a `FROM` statement similar to the following:

```
# SageMaker PyTorch image
FROM 763104351884.dkr.ecr.us-east-1.amazonaws.com/pytorch-training:1.7.1-gpu-py36-cu110-ubuntu18.04

# Add your dependencies here
RUN ...

ENV PATH="/opt/ml/code:${PATH}"

# this environment variable is used by the SageMaker PyTorch container to determine our user code directory.
ENV SAGEMAKER_SUBMIT_DIRECTORY /opt/ml/code

# /opt/ml and all subdirectories are utilized by SageMaker, use the /code subdirectory to store your user code.
COPY cifar10.py /opt/ml/code/cifar10.py
```

Additionally, when you define a PyTorch or TensorFlow `estimator`, you must specify that the `entry_point` for your training script\. This should be the same path identified with `ENV SAGEMAKER_SUBMIT_DIRECTORY` in your Dockerfile\. 

You must push this docker container to Amazon Elastic Container Registry \(Amazon ECR\) and use the image URI \(`image_uri`\) to define an estimator\. See [Extend a Prebuilt Container](https://docs.aws.amazon.com/sagemaker/latest/dg/prebuilt-containers-extend.html) for more information\. 

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