# Use the SageMaker distributed data parallel API<a name="data-parallel-use-api"></a>

## Use distributed data parallel with SageMaker<a name="data-parallel-use-api-overview"></a>

 To use distributed data parallel \(SDP\) with SageMaker, you start with a training script\. Just like any training job on SageMaker, you use your training script to launch an SageMaker training job\. To get started, it is recommended that you use SDP on SageMaker in the following ways: 
+  Using a [SageMaker notebook instance](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) 
+  Using [SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html) 

 With either option, you can then use the [Distributed Training Jupyter Notebok Examples](distributed-training-notebook-examples.md) provided with this documentation to get started\.  

## SageMaker Python SDK's SDP APIs<a name="data-parallel-use-python-skd-api"></a>

To use the SageMaker data parallel \(SDP\) library, you must create a training script for one of the supported frameworks and launch the training job using the SageMaker Python SDK\. To learn how you can incorporate SMP into a training script, see [Modify Your Training Script to Use SMP](model-parallel-customize-training-script.md)\. The SDP API documentation is located in the SageMaker Python SDK\. Refer to [SDP API documentation](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html)\. 

SageMaker supports the following training environment configurations\.
+ You can use a prebuilt TensorFlow or PyTorch container\.
+ You can customize SageMaker prebuilt containers or extend them to handle any additional functional requirements for your algorithm or model that the prebuilt SageMaker Docker image doesn't support\. To see an example of how you can extend a pre\-built container, see [Extend a Prebuilt Container](https://docs.aws.amazon.com/sagemaker/latest/dg/prebuilt-containers-extend.html)\.

  For this option, use a [Deep Learning Container URL](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) in your Dockerfile\. 

  For example, if you were using PyTorch 1\.6\.0, your Dockerfile should contain a `FROM` statement similar to the following:

  ```
  # SageMaker PyTorch image
  FROM 763104351884.dkr.ecr.us-east-1.amazonaws.com/pytorch-training:1.5.1-gpu-py36-cu101-ubuntu16.04
  
  ENV PATH="/opt/ml/code:${PATH}"
  
  # this environment variable is used by the SageMaker PyTorch container to determine our user code directory.
  ENV SAGEMAKER_SUBMIT_DIRECTORY /opt/ml/code
  
  # /opt/ml and all subdirectories are utilized by SageMaker, use the /code subdirectory to store your user code.
  COPY cifar10.py /opt/ml/code/cifar10.py
  
  # Defines cifar10.py as script entrypoint
  ENV SAGEMAKER_PROGRAM cifar10.py
  ```
+ You can adapt your owner docker container to work with SageMaker using the [SageMaker Training toolkit](https://github.com/aws/sagemaker-training-toolkit)\. For an example, see [Adapting Your Own Training Container](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html)\.

In all cases, you launch your job using an SageMaker Python SDK `Estimator`\. 

If you are using the SageMaker pre\-built PyTorch or TensorFlow containers, see the following section to learn how configure an `Estimator` to use SDP\. 

If you bring your own container or extend a pre\-built container, you can create an instance of the `Estimator` class with desired Docker image\.

## TensorFlow estimator<a name="data-parallel-tensorflow-api"></a>

 *class *`sagemaker.tensorflow.estimator.TensorFlow(`*py\_version=None*`,`*framework\_version=None*`,`*model\_dir=None*`,`*image\_uri=None*`,`*distribution=None*`,`*kwargs*`)` 

 You can use the [SDP API](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html) by specifying `smdistributed dataparallel` as the `distribution` strategy in SageMaker TensorFlow Estimator API\. Please refer to [SageMaker TensorFlow Estimator API specification](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) for full details of the API\. 

 The following two parameters of the SageMaker Python SDK TensorFlow Estimator are required to use `smdistributed.dataparallel` with TensorFlow\. 

 `distribution(dict)(optional):`  A dictionary with information on how to run distributed training \(default: None\)\.  
+  To use `smdistributed.dataparallel` as distribution strategy, use the following option: 
+  distribution=\{"smdistributed": \{                     "dataparallel": \{                             "enabled": True                     \}                \}              \} 

 `train_instance_type (str)(required)`: Type of EC2 instance to use\. 
+  If `smdistributed.dataparallel` distribution strategy is used, allowed instance types are: `ml.p4dn.24xlarge`, `ml.p3dn.24xlarge` and `ml.p3.16xlarge`  

 **Example** 

```
from sagemaker.tensorflow import TensorFlow

tf_estimator = TensorFlow(
                          base_job_name = "training_job_name_prefix",
                          entry_point="tf-train.py",
                          role="SageMakerRole",
                          image_uri="smdistributed_dataparallel_tensorflow_dlc_image_uri",
                          framework_version="2.3",
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
 If you are using SageMaker Python SDK 1\.x, you need to use `hyperparameters` to specify to use `smdistributed dataparallel` as the distributed training strategy\. 

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

## PyTorch estimator<a name="data-parallel-pytorch-api"></a>

 *class *`sagemaker.pytorch.estimator.PyTorch(`*py\_version=None*`,`*framework\_version=None*`,`*model\_dir=None*`,`*image\_uri=None*`, hyperparameters=None, `*distribution=None*`,`*kwargs*`)` 

 ​ 

 You can use the [SDP API](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html) by specifying `smdistributed dataparallel` as the `distribution` strategy in SageMaker PyTorch Estimator API\. Please refer to the [SageMaker PyTorch Estimator API specification](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html#pytorch-estimator) for full details of the API\. 

 Below 2 parameters of the SageMaker PyTorch Estimator are necessary for using `smdistributed.dataparallel` on SageMaker with PyTorch\. 

 `distribution(dict)(optional):`  A dictionary with information on how to run distributed training \(default: None\)\.  
+  To use`smdistributed.dataparallel` as distribution strategy, use the following option: 
+  distribution=\{"smdistributed": \{                     "dataparallel": \{                             "enabled": True                     \}                \}              \} 

 `train_instance_type (str)(required)`: Type of EC2 instance to use\. 
+  If `smdistributed.dataparallel` distribution strategy is used, allowed instance types are: `ml.p4dn.24xlarge`, `ml.p3dn.24xlarge` and `ml.p3.16xlarge`  

 **Example** 

```
from sagemaker.pytorch import PyTorch
pt_estimator = PyTorch(
                       base_job_name="training_job_name_prefix",
                       entry_point="pt-train.py",
                       role="SageMakerRole",
                       image_uri="smdistributed_dataparallel_pytorch_dlc_image_uri",
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
 If you are using SageMaker Python SDK 1\.x, you need to use `hyperparameters` to specify to use `smdistributed.dataparallel` as the distributed training strategy\. 

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