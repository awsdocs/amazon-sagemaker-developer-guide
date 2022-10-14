# Train Using a Heterogeneous Cluster<a name="train-heterogeneous-cluster"></a>

Using the heterogeneous cluster feature of SageMaker Training, you can run a training job with multiple types of ML instances for a better resource scaling and utilization for different ML training tasks and purposes\. For example, if your training job on a cluster with GPU instances suffers low GPU utilization and CPU bottleneck problems due to CPU\-intensive tasks, using a heterogeneous cluster can help offload CPU\-intensive tasks by adding more cost\-efficient CPU instance groups, resolve such bottleneck problems, and achieve a better GPU utilization\.

**Note**  
This feature is available in the SageMaker Python SDK v2\.98\.0 and later\.

**Note**  
This feature is available through the SageMaker [PyTorch](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html) and [TensorFlow](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) framework estimator classes\. Supported frameworks are PyTorch v1\.10 or later and TensorFlow v2\.6 or later\.

**Topics**
+ [How to Configure a Heterogeneous Cluster](#train-heterogeneous-cluster-configure)
+ [Distributed Training with a Heterogeneous Cluster](#train-heterogeneous-cluster-configure-distributed)
+ [Modify Your Training Script to Assign Instance Groups](#train-heterogeneous-cluster-modify-training-script)
+ [Considerations](#train-heterogeneous-cluster-considerations)

## How to Configure a Heterogeneous Cluster<a name="train-heterogeneous-cluster-configure"></a>

This section provides instructions on how to run a training job using a heterogeneous cluster that consists of multiple instance types\.

**Topics**
+ [Using the SageMaker Python SDK](#train-heterogeneous-cluster-configure-pysdk)
+ [Using the Low\-Level SageMaker APIs](#train-heterogeneous-cluster-configure-api)

### Using the SageMaker Python SDK<a name="train-heterogeneous-cluster-configure-pysdk"></a>

Follow instructions on how to configure instance groups for a heterogeneous cluster using the SageMaker Python SDK\.

1. To configure instance groups of a heterogeneous cluster for a training job, use the `sagemaker.instance_group.InstanceGroup` class\. You can specify a custom name for each instance group, the instance type, and the number of instances for each instance group\. For more information, see [sagemaker\.instance\_group\.InstanceGroup](https://sagemaker.readthedocs.io/en/stable/api/utility/instance_group.html) in the *SageMaker Python SDK documentation*\.
**Note**  
For more information about available instance types and the maximum number of instance groups that you can configure in a heterogeneous cluster, see the [ InstanceGroup](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InstanceGroup.html) API reference\.

   The following code example shows how to set up two instance groups that consists of two `ml.c5.18xlarge` CPU\-only instances named `instance_group_1` and one `ml.p3dn.24xlarge` GPU instance named `instance_group_2`, as shown in the following diagram\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/HCTraining.png)

   ```
   from sagemaker.instance_group import InstanceGroup
   
   instance_group_1 = InstanceGroup(
       "instance_group_1", "ml.c5.18xlarge", 2
   )
   instance_group_2 = InstanceGroup(
       "instance_group_2", "ml.p3dn.24xlarge", 1
   )
   ```

1. Using the instance group objects, set up training input channels and assign instance groups to the channels through the `instance_group_names` argument of the [sagemaker\.inputs\.TrainingInput](https://sagemaker.readthedocs.io/en/stable/api/utility/inputs.html) class\. The `instance_group_names` argument accepts a list of strings of instance group names\.

   The following example shows how to set two training input channels and assign the instance groups created in the example of the previous step\. You can also specify Amazon S3 bucket paths to the `s3_data` argument for the instance groups to process data for your usage purposes\.

   ```
   from sagemaker.inputs import TrainingInput
   
   training_input_channel_1 = TrainingInput(
       s3_data_type='S3Prefix', # Available Options: S3Prefix | ManifestFile | AugmentedManifestFile
       s3_data='s3://your-training-data-storage/folder1',
       distribution='FullyReplicated', # Available Options: FullyReplicated | ShardedByS3Key 
       input_mode='File', # Available Options: File | Pipe | FastFile
       instance_group_names=["instance_group_1"],
   )
   
   training_input_channel_2 = TrainingInput(
       s3_data_type='S3Prefix',
       s3_data='s3://your-training-data-storage/folder2',
       distribution='FullyReplicated',
       input_mode='File',
       instance_group_names=["instance_group_2"],
   )
   ```

   For more information about the arguments of `TrainingInput`, see the following links\.
   + The [sagemaker\.inputs\.TrainingInput](https://sagemaker.readthedocs.io/en/stable/api/utility/inputs.html) class in the *SageMaker Python SDK documentation*
   + The [S3DataSource](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_S3DataSource.html) API in the *SageMaker API Reference*

1. Configure a SageMaker estimator with the `instance_groups` argument as shown in the following code example\. The `instance_groups` argument accepts a list of `InstanceGroup` objects\.

------
#### [ PyTorch ]

   ```
   from sagemaker.pytorch import PyTorch
   
   estimator = PyTorch(
       ...
       entry_point='my-training-script.py',
       framework_version='x.y.z',    # 1.10.0 or later
       py_version='pyxy',            
       job_name='my-training-job-with-heterogeneous-cluster',
       instance_groups=[instance_group_1, instance_group_2]
   )
   ```

------
#### [ TensorFlow ]

   ```
   from sagemaker.tensorflow import TensorFlow
   
   estimator = TensorFlow(
       ...
       entry_point='my-training-script.py',
       framework_version='x.y.z', # 2.6.0 or later
       py_version='pyxy',
       job_name='my-training-job-with-heterogeneous-cluster',
       instance_groups=[instance_group_1, instance_group_2]
   )
   ```

------
**Note**  
The `instance_type` and `instance_count` argument pair and the `instance_groups` argument of the SageMaker estimator class are mutually exclusive\. For homogeneous cluster training, use the `instance_type` and `instance_count` argument pair\. For heterogeneous cluster training, use `instance_groups`\.
**Note**  
To find a complete list of available framework containers, framework versions, and Python versions, see [SageMaker Framework Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-framework-containers-sm-support-only) in the AWS Deep Learning Container GitHub repository\.

1. Configure the `estimator.fit` method with the training input channels configured with the instance groups and start the training job\.

   ```
   estimator.fit(
       inputs={
           'training': training_input_channel_1, 
           'dummy-input-channel': training_input_channel_2
       }
   )
   ```

### Using the Low\-Level SageMaker APIs<a name="train-heterogeneous-cluster-configure-api"></a>

If you use the AWS Command Line Interface or AWS SDK for Python \(Boto3\) and want to use low\-level SageMaker APIs for submitting a training job request with a heterogeneous cluster, see the following API references\.
+ [CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)
+ [ResourceConfig ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResourceConfig.html)
+ [InstanceGroup](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InstanceGroup.html)
+ [S3DataSource](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_S3DataSource.html)

## Distributed Training with a Heterogeneous Cluster<a name="train-heterogeneous-cluster-configure-distributed"></a>

Through the `distribution` argument of the SageMaker estimator class, you can assign a specific instance group to run distributed training\. For example, assume that you have the following two instance groups and want to run multi\-GPU training on one of them\. 

```
from sagemaker.instance_group import InstanceGroup

instance_group_1 = InstanceGroup("instance_group_1", "ml.c5.18xlarge", 1)
instance_group_2 = InstanceGroup("instance_group_2", "ml.p3dn.24xlarge", 2)
```

You can set the distributed training configuration for one of the instance groups\. For example, the following code examples show how to assign `training_group_2` with two `ml.p3dn.24xlarge` instances to the distributed training configuration\.

**Note**  
Currently, only one instance group of a heterogeneous cluster can be specified to the distribution configuration\.

**With MPI**

------
#### [ PyTorch ]

```
from sagemaker.pytorch import PyTorch

estimator = PyTorch(
    ...
    instance_groups=[instance_group_1, instance_group_2],
    distribution={
        "mpi": {
            "enabled": True, "processes_per_host": 8
        },
        "instance_groups": [instance_group_2]
    }
)
```

------
#### [ TensorFlow ]

```
from sagemaker.tensorflow import TensorFlow

estimator = TensorFlow(
    ...
    instance_groups=[instance_group_1, instance_group_2],
    distribution={
        "mpi": {
            "enabled": True, "processes_per_host": 8
        },
        "instance_groups": [instance_group_2]
    }
)
```

------

**With the SageMaker data parallel library**

------
#### [ PyTorch ]

```
from sagemaker.pytorch import PyTorch

estimator = PyTorch(
    ...
    instance_groups=[instance_group_1, instance_group_2],
    distribution={
        "smdistributed": {
            "dataparallel": {
                "enabled": True
            }
        }, 
        "instance_groups": [instance_group_2]
    }
)
```

------
#### [ TensorFlow ]

```
from sagemaker.tensorflow import TensorFlow

estimator = TensorFlow(
    ...
    instance_groups=[instance_group_1, instance_group_2],
    distribution={
        "smdistributed": {
            "dataparallel": {
                "enabled": True
            }
        }, 
        "instance_groups": [instance_group_2]
    }
)
```

------

**Note**  
When using the SageMaker data parallel library, make sure the instance group consists of the [supported instance types by the library](https://docs.aws.amazon.com/sagemaker/latest/dg/distributed-data-parallel-support.html#distributed-data-parallel-supported-instance-types)\. 

For more information about the SageMaker data parallel library, see [SageMaker Data Parallel Training](https://docs.aws.amazon.com/sagemaker/latest/dg/data-parallel.html)\.

**With the SageMaker model parallel library**

------
#### [ PyTorch ]

```
from sagemaker.pytorch import PyTorch

estimator = PyTorch(
    ...
    instance_groups=[instance_group_1, instance_group_2],
    distribution={
        "smdistributed": {
            "modelparallel": {
                "enabled":True,
                "parameters": {
                    ...   # SageMaker model parallel parameters
                } 
            }
        }, 
        "instance_groups": [instance_group_2]
    }
)
```

------
#### [ TensorFlow ]

```
from sagemaker.tensorflow import TensorFlow

estimator = TensorFlow(
    ...
    instance_groups=[instance_group_1, instance_group_2],
    distribution={
        "smdistributed": {
            "modelparallel": {
                "enabled":True,
                "parameters": {
                    ...   # SageMaker model parallel parameters
                } 
            }
        }, 
        "instance_groups": [instance_group_2]
    }
)
```

------

For more information about the SageMaker model parallel library, see [SageMaker Model Parallel Training](https://docs.aws.amazon.com/sagemaker/latest/dg/model-parallel.html)\.

## Modify Your Training Script to Assign Instance Groups<a name="train-heterogeneous-cluster-modify-training-script"></a>

With the heterogeneous cluster configuration in the previous sections, you have prepared the SageMaker training environment and instances for your training job\. To further assign the instance groups to certain training and data processing tasks, the next step is to modify your training script\. By default, the training job simply makes training script replicas for all nodes regardless the size of the instance, and this might lead to performance loss\. 

For example, if you mix CPU instances and GPU instances in a heterogeneous cluster while passing a deep neural network training script to the `entry_point` argument of the SageMaker estimator, the `entry_point` script is replicated to each instance\. This means that, without proper task assignments, CPU instances also run the entire script and start the training job that’s designed for distributed training on GPU instances\. Therefore, you must make changes in specific processing functions that you want to offload and run on the CPU instances\. You can use the SageMaker environment variables to retrieve the information of the heterogeneous cluster and let specific processes to run accordingly\.

### Query instance group information during the initialization phase of a SageMaker training job<a name="train-heterogeneous-cluster-modify-training-script-query-env-var"></a>

When your training job starts, your training script reads SageMaker training environment information that includes heterogeneous cluster configuration\. The configuration contains information such as the current instance groups, the current hosts in each group, and in which group the current host resides\.

You can retrieve instance group information in the following ways\.

**\(Recommended\) Reading instance group information with the SageMaker training toolkit**

Use the environment Python module that the [SageMaker training toolkit library](https://github.com/aws/sagemaker-training-toolkit) provides\. The toolkit library is preinstalled in the [SageMaker framework containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-framework-containers-sm-support-only) for TensorFlow and PyTorch, so you don’t need an additional installation step when using the prebuilt containers\. This is the recommended way to retrieve the SageMaker environment variables with fewer code changes in your training script\.

```
from sagemaker_training import environment

env = environment.Environment()
```

Environment variables related to general SageMaker training and heterogeneous clusters:
+ `env.is_hetero` – Returns a Boolean result whether a heterogeneous cluster is configured or not\.
+ `env.current_host` – Returns the current host\.
+ `env.current_instance_type` – Returns the type of instance of the current host\.
+ `env.current_instance_group` – Returns the name of the current instance group\.
+ `env.current_instance_group_hosts` – Returns a list of hosts in current instance group\.
+ `env.instance_groups` – Returns a list of instance group names used for training\.
+ `env.instance_groups_dict` – Returns the entire heterogeneous cluster configuration of the training job\.
+ `env.distribution_instance_groups` – Returns a list of instance groups assigned to the `distribution` parameter of the SageMaker estimator class\.
+ `env.distribution_hosts` – Returns a list of hosts belonging to the instance groups assigned to the `distribution` parameter of the SageMaker estimator class\.

For example, consider the following example of a heterogeneous cluster that consists of two instance groups\.

```
from sagemaker.instance_group import InstanceGroup

instance_group_1 = InstanceGroup(
    "instance_group_1", "ml.c5.18xlarge", 1)
instance_group_2 = InstanceGroup(
    "instance_group_2", "ml.p3dn.24xlarge", 2)
```

The output of `env.instance_groups_dict` of the example heterogeneous cluster should be similar to the following\.

```
{
    "instance_group_1": {
        "hosts": [
            "algo-2"
        ],
        "instance_group_name": "instance_group_1",
        "instance_type": "ml.c5.18xlarge"
    },
    "instance_group_2": {
        "hosts": [
            "algo-3",
            "algo-1"
        ],
        "instance_group_name": "instance_group_2",
        "instance_type": "ml.p3dn.24xlarge"
    }
}
```

**\(Optional\) Reading instance group information from the resource configuration JSON file**

If you prefer to retrieve the environment variables in JSON format, you can directly use the resource configuration JSON file\. The JSON file in a SageMaker training instance is located at `/opt/ml/input/config/resourceconfig.json` by default\.

```
file_path = '/opt/ml/input/config/resourceconfig.json'
config = read_file_as_json(file_path)
print(json.dumps(config, indent=4, sort_keys=True))
```

## Considerations<a name="train-heterogeneous-cluster-considerations"></a>

Consider the following items when using the heterogeneous cluster feature\.
+ All instance groups share the same Docker image and training script\. Therefore, your training script should be modified to detect which instance group it belongs to and fork execution accordingly\.
+ The heterogeneous cluster feature is not supported in SageMaker local mode\.
+ The Amazon CloudWatch log streams of a heterogeneous cluster training job are not grouped by instance groups\. You need to figure out from the logs which nodes are in which group\.
+ The heterogeneous cluster feature is available through the SageMaker [PyTorch](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html) and [TensorFlow](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) framework estimator classes\. Supported frameworks are PyTorch v1\.10 or later and TensorFlow v2\.6 or later\. To find a complete list of available framework containers, framework versions, and Python versions, see [SageMaker Framework Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-framework-containers-sm-support-only) in the AWS Deep Learning Container GitHub repository\.
+ A distributed training strategy can be applied only to one instance group\.