# Use the SageMaker Distributed Data Parallel API<a name="data-parallel-use-api"></a>

Use this page to learn how you can import and use SageMaker's distributed data parallel library\. For API specifications, refer to [distributed data parallel](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html) in the SageMaker Python SDK documentation\.

## Use SageMaker's Distributed Data Parallel Library<a name="data-parallel-use-api-overview"></a>

To use SageMaker's distributed data parallel library, you start with a training script\. Just as with any training job on SageMaker, you use your training script to launch a SageMaker training job\. To get started, we recommend that you use the SageMaker data parallel library in the following ways:
+  Using a [SageMaker notebook instance](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) 
+  Using [SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html) 

With either option, you can then use the [Distributed Training Jupyter Notebook Examples](distributed-training-notebook-examples.md) provided with this documentation to get started\.  

## Use the Data Parallel Library with SageMaker's Python SDK<a name="data-parallel-use-python-skd-api"></a>

To use SageMaker's distributed data parallel library, you must create a training script for one of the supported frameworks and launch the training job using the SageMaker Python SDK\. To learn how you can incorporate the library into a training script, see [Modify Your Training Script Using SageMaker's Distributed Model Parallel Library](model-parallel-customize-training-script.md)\. The library API documentation is located in the SageMaker Python SDK\. See SageMaker's [data parallel API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html)\. 

SageMaker supports the following training environment configurations:
+ You can use a prebuilt TensorFlow or PyTorch container\.
+ You can customize SageMaker prebuilt containers or extend them to handle any additional functional requirements for your algorithm or model that the prebuilt SageMaker Docker image doesn't support\. For an example of how you can extend a pre\-built container, see [Extend a Prebuilt Container](https://docs.aws.amazon.com/sagemaker/latest/dg/prebuilt-containers-extend.html)\.

  To extend a pre\-built container, or adapt your own container to use the library, you must use one of the following PyTorch or TensorFlow GPU general framework base\-images:
  + 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/tensorflow\-training:2\.4\.1\-gpu\-py37\-cu110\-ubuntu18\.04
  + 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/tensorflow\-training:2\.3\.1\-gpu\-py37\-cu110\-ubuntu18\.04
  + 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/pytorch\-training:1\.8\.0\-gpu\-py36\-cu111\-ubuntu18\.04
  + 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/pytorch\-training:1\.7\.1\-gpu\-py36\-cu110\-ubuntu18\.04
  + 763104351884\.dkr\.ecr\.*<region>*\.amazonaws\.com/pytorch\-training:1\.6\.0\-gpu\-py36\-cu110\-ubuntu18\.04

  For example, if you are using PyTorch 1\.6\.0, your Dockerfile should contain a `FROM` statement similar to the following:

  ```
  # SageMaker PyTorch image
  FROM 763104351884.dkr.ecr.us-east-1.amazonaws.com/pytorch-training:1.6.0-gpu-py36-cu110-ubuntu18.04
  
  ENV PATH="/opt/ml/code:${PATH}"
  
  # this environment variable is used by the SageMaker PyTorch container to determine our user code directory.
  ENV SAGEMAKER_SUBMIT_DIRECTORY /opt/ml/code
  
  # /opt/ml and all subdirectories are utilized by SageMaker, use the /code subdirectory to store your user code.
  COPY cifar10.py /opt/ml/code/cifar10.py
  
  # Defines cifar10.py as script entrypoint
  ENV SAGEMAKER_PROGRAM cifar10.py
  ```
+ You can adapt your own Docker container to work with SageMaker using the [SageMaker Training toolkit](https://github.com/aws/sagemaker-training-toolkit)\. For an example, see [Adapting Your Own Training Container](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html)\.

In all cases, you launch your job using a SageMaker Python SDK `Estimator`\.

If you are using the SageMaker pre\-built PyTorch or TensorFlow containers, see the following section to learn how configure an `Estimator` to use the library\.

If you bring your own container or extend a pre\-built container, you can create an instance of the `Estimator` class with the desired Docker image\.

## TensorFlow Estimator<a name="data-parallel-tensorflow-api"></a>

You can use SageMaker's [distributed data parallel library API](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html) by specifying `smdistributed dataparallel` as the `distribution` strategy in the SageMaker TensorFlow Estimator API\. Refer to [SageMaker TensorFlow Estimator API documentation](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) for full details about the API\.

*class *`sagemaker.tensorflow.estimator.TensorFlow(`*py\_version=None*`,`*framework\_version=None*`,`*model\_dir=None*`,`*image\_uri=None*`,`*distribution=None*`,`*kwargs*`)`

The following two parameters of the SageMaker Python SDK TensorFlow estimator are required to use `smdistributed.dataparallel` with TensorFlow\. 

**`distribution(dict)(optional)`**: A dictionary with information on how to run distributed training \(default: `None`\)\.  
+ To use `smdistributed.dataparallel` as a distribution strategy, use the following option: 

  ```
  distribution = { "smdistributed": { "dataparallel": { "enabled": True } } } 
  ```

 **`train_instance_type (str)(required)`**: A type of Amazon EC2 instance to use\. 
+ If the `smdistributed.dataparallel` distribution strategy is used, the allowed instance types are: `ml.p4d.24xlarge`, `ml.p3dn.24xlarge`, and `ml.p3.16xlarge`\.

 **Example** 

```
from sagemaker.tensorflow import TensorFlow

tf_estimator = TensorFlow(
                          base_job_name = "training_job_name_prefix",
                          entry_point="tf-train.py",
                          role="SageMakerRole",
                          framework_version="2.3",
                          # You must set py_version to py36
                          py_version='py36',
                          # For training with multi node distributed training, set this count.
                          # Example: 2,3,4,..8
                          instance_count=1,
                          # For training with p3dn instance use - ml.p3dn.24xlarge
                          instance_type="ml.p3.16xlarge",
                          # Training using smdistributed.dataparallel Distributed Training Framework
                          distribution={"smdistributed": {
                                            "dataparallel": {
                                                "enabled": True
                                             }
                                         }
                                       }
                         )
tf_estimator.fit("s3://bucket/path/to/training/data")
```

**Note**  
If you are using the SageMaker Python SDK 1\.x, you need to use `hyperparameters` to specify to use `smdistributed dataparallel` as the distributed training strategy\. 

```
from sagemaker.tensorflow import TensorFlow
tf_estimator = TensorFlow(
                          ...
                          # Training using smdistributed.dataparallel Distributed Training Framework
                          hyperparameters={"sagemaker_distributed_dataparallel_enabled": True},
                          ...
                         )
tf_estimator.fit("s3://bucket/path/to/training/data")
```

## PyTorch Estimator<a name="data-parallel-pytorch-api"></a>

You can use SageMaker's [distributed data parallel library API](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html) by specifying `smdistributed dataparallel` as the `distribution` strategy in SageMaker PyTorch Estimator API\. Refer to the [SageMaker PyTorch Estimator API specification](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html#pytorch-estimator) for full details of the API\. 

*class *`sagemaker.pytorch.estimator.PyTorch(`*py\_version=None*`,`*framework\_version=None*`,`*model\_dir=None*`,`*image\_uri=None*`, hyperparameters=None, `*distribution=None*`,`*kwargs*`)`

The following two parameters of the SageMaker PyTorch `Estimator` are necessary for using `smdistributed.dataparallel` on SageMaker with PyTorch\. 

`distribution (dict)(optional):`  A dictionary with information on how to run distributed training \(default: `None`\)\.  
+ To use`smdistributed.dataparallel` as a distribution strategy, use the following option: 

  ```
  distribution={"smdistributed": { "dataparallel": { "enabled": True } } } 
  ```

 **`train_instance_type (str)(required)`**: A type of Amazon EC2 instance to use\. 
+ If the `smdistributed.dataparallel` distribution strategy is used, the allowed instance types are: `ml.p4d.24xlarge`, `ml.p3dn.24xlarge`, and `ml.p3.16xlarge`\.

 **Example** 

```
from sagemaker.pytorch import PyTorch
pt_estimator = PyTorch(
                       base_job_name="training_job_name_prefix",
                       entry_point="pt-train.py",
                       role="SageMakerRole",
                       # You must set py_version to py36
                       py_version='py36',
                       framework_version='1.6.0',
                       # For training with multi node distributed training, set this count.
                       # Example: 2,3,4,..8
                       instance_count=1,
                       # For training with p3dn instance use - ml.p3dn.24xlarge
                       instance_type="ml.p3.16xlarge",
                       # Training using smdistributed.dataparallel Distributed Training Framework
                       distribution={"smdistributed": {
                                            "dataparallel": {
                                                "enabled": True
                                            }
                                       }
                                    }
                      )
pt_estimator.fit("s3://bucket/path/to/training/data")                       
```

**Note**  
If you are using SageMaker Python SDK 1\.x, you need to use `hyperparameters` to specify `smdistributed.dataparallel` as the distributed training strategy\. 

```
from sagemaker.pytorch import PyTorch
pt_estimator = PyTorch(
                       ...
                       # Training using smdistributed.dataparallel Distributed Training Framework
                       hyperparameters={"sagemaker_distributed_dataparallel_enabled": True},
                       ...
                      )
pt_estimator.fit("s3://bucket/path/to/training/data")                       
```