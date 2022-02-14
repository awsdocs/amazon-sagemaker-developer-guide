# How Amazon SageMaker Provides Training Information<a name="your-algorithms-training-algo-running-container"></a>

This section explains how SageMaker makes training information, such as training data, hyperparameters, and other configuration information, available to your Docker container\. 

When you send a [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request to SageMaker to start model training, you specify the Amazon Elastic Container Registry \(Amazon ECR\) path of the Docker image that contains the training algorithm\. You also specify the Amazon Simple Storage Service \(Amazon S3\) location where training data is stored and algorithm\-specific parameters\. SageMaker makes this information available to the Docker container so that your training algorithm can use it\. This section explains how we make this information available to your Docker container\. For information about creating a training job, see `CreateTrainingJob`\. For more information on the way that SageMaker containers organize information, see [Using the SageMaker Training and Inference Toolkits ](amazon-sagemaker-toolkits.md)\.

**Topics**
+ [Hyperparameters](#your-algorithms-training-algo-running-container-hyperparameters)
+ [Environment Variables](#your-algorithms-training-algo-running-container-environment-variables)
+ [Input Data Configuration](#your-algorithms-training-algo-running-container-inputdataconfig)
+ [Training Data](#your-algorithms-training-algo-running-container-trainingdata)
+ [Distributed Training Configuration](#your-algorithms-training-algo-running-container-dist-training)

## Hyperparameters<a name="your-algorithms-training-algo-running-container-hyperparameters"></a>

 SageMaker makes the hyperparameters in a `CreateTrainingJob` request available in the Docker container in the `/opt/ml/input/config/hyperparameters.json` file\.

## Environment Variables<a name="your-algorithms-training-algo-running-container-environment-variables"></a>

The following environment variables are set in the container:
+ TRAINING\_JOB\_NAME – Specified in the `TrainingJobName` parameter of the `CreateTrainingJob` request\.
+ TRAINING\_JOB\_ARN – The Amazon Resource Name \(ARN\) of the training job returned as the `TrainingJobArn` in the `CreateTrainingJob` response\.
+ All environment variables specified in the [Environment](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html#sagemaker-CreateTrainingJob-request-Environment) parameter in the `CreateTrainingJob` request\.

## Input Data Configuration<a name="your-algorithms-training-algo-running-container-inputdataconfig"></a>

You specify data channel information in the `InputDataConfig` parameter in a `CreateTrainingJob` request\. SageMaker makes this information available in the `/opt/ml/input/config/inputdataconfig.json` file in the Docker container\.

For example, suppose that you specify three data channels \(`train`, `evaluation`, and `validation`\) in your request\. SageMaker provides the following JSON:

```
{
  "train" : {"ContentType":  "trainingContentType",
             "TrainingInputMode": "File",
             "S3DistributionType": "FullyReplicated",
             "RecordWrapperType": "None"},
  "evaluation" : {"ContentType":  "evalContentType",
                  "TrainingInputMode": "File",
                  "S3DistributionType": "FullyReplicated",
                  "RecordWrapperType": "None"},
  "validation" : {"TrainingInputMode": "File",
                  "S3DistributionType": "FullyReplicated",
                  "RecordWrapperType": "None"}
}
```

**Note**  
SageMaker provides only relevant information about each data channel \(for example, the channel name and the content type\) to the container, as shown\. `S3DistributionType` will be set as `FullyReplicated` if specify EFS or FSxLustre as input data sources\.

## Training Data<a name="your-algorithms-training-algo-running-container-trainingdata"></a>

The `TrainingInputMode` parameter in the `AlgorithmSpecification` of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request specifies how the training dataset is made available\. The following input modes are available:
+ **`File` mode**
  + `TrainingInputMode` parameter written to `inputdataconfig.json`: "File"
  + Data channel directory in the Docker container: `/opt/ml/input/data/channel_name`
  + Supported data sources: Amazon Simple Storage Service \(Amazon S3\), Amazon Elastic File System \(Amazon EFS\), and Amazon FSx for Lustre

  A directory is created for each channel\. For example, if you have three channels named `training`, `validation`, and `testing`, SageMaker makes three directories in the Docker container: 
  + `/opt/ml/input/data/training`
  + `/opt/ml/input/data/validation`
  + `/opt/ml/input/data/testing`
**Note**  
Channels that use file system data sources such as Amazon EFS and Amazon FSx must use `File` mode\. If a file system is specified, the directory path provided in the channel is mounted at `/opt/ml/input/data/channel_name`\.
+ **`FastFile` mode**
  + `TrainingInputMode` parameter written to `inputdataconfig.json`: "FastFile"
  + Data channel directory in the Docker container: `/opt/ml/input/data/channel_name`
  + Supported data sources: Amazon S3

  The channel directory is mounted as read\-only\.

  Algorithms that support `File` mode can seamlessly work with `FastFile` mode with no code changes\.
**Note**  
Channels that use `FastFile` mode must use a `S3DataType` of "S3Prefix"\.  
`FastFile` mode presents a folder view that uses the forward slash \(`/`\) as the delimiter for grouping Amazon S3 objects into folders\. `S3Uri` prefixes must not correspond to a partial folder name\. For example, if an Amazon S3 dataset contains `s3://my-bucket/train-01/data.csv`, then neither `s3://my-bucket/train` nor `s3://my-bucket/train-01` are allowed as `S3Uri` prefixes\.  
A trailing forward slash is recommended to define a channel corresponding to a folder\. For example, the `s3://my-bucket/train-01/` channel for the `train-01` folder\. Without the trailing forward slash, the channel would be ambiguous if there existed another folder `s3://my-bucket/train-011/` or file `s3://my-bucket/train-01.txt/`\.
+ **`Pipe` mode**
  + `TrainingInputMode` parameter written to `inputdataconfig.json`: "Pipe"
  + Data channel directory in the Docker container: `/opt/ml/input/data/channel_name_epoch_number`
  + Supported data sources: Amazon S3

  You need to read from a separate pipe for each channel\. For example, if you have three channels named `training`, `validation`, and `testing`, you need to read from the following pipes:
  + `/opt/ml/input/data/training_0, /opt/ml/input/data/training_1, ...`
  + `/opt/ml/input/data/validation_0, /opt/ml/input/data/validation_1, ...`
  + `/opt/ml/input/data/testing_0, /opt/ml/input/data/testing_1, ...`

  Read the pipes sequentially\. For example, if you have a channel called `training`, read the pipes in this sequence: 

  1. Open `/opt/ml/input/data/training_0` in read mode and read it to end\-of\-file \(EOF\) or, if you are done with the first epoch, close the pipe file early\. 

  1. After closing the first pipe file, look for `/opt/ml/input/data/training_1` and read it until you have completed the second epoch, and so on\.

  If the file for a given epoch doesn't exist yet, your code may need to retry until the pipe is created There is no sequencing restriction across channel types\. For example, you can read multiple epochs for the `training` channel and only start reading the `validation` channel when you are ready\. Or, you can read them simultaneously if your algorithm requires that\.

## Distributed Training Configuration<a name="your-algorithms-training-algo-running-container-dist-training"></a>

If you're performing distributed training with multiple containers, SageMaker makes information about all containers available in the `/opt/ml/input/config/resourceconfig.json` file\.

To enable inter\-container communication, this JSON file contains information for all containers\. SageMaker makes this file available for both `File` and `Pipe` mode algorithms\. The file provides the following information:
+ `current_host`—The name of the current container on the container network\. For example, `algo-1`\. Host values can change at any time\. Don't write code with specific values for this variable\.
+ `hosts`—The list of names of all containers on the container network, sorted lexicographically\. For example, `["algo-1", "algo-2", "algo-3"]` for a three\-node cluster\. Containers can use these names to address other containers on the container network\. Host values can change at any time\. Don't write code with specific values for these variables\.
+ `network_interface_name`—The name of the network interface that is exposed to your container\. For example, containers running the Message Passing Interface \(MPI\) can use this information to set the network interface name\.
+ Do not use the information in `/etc/hostname` or `/etc/hosts` because it might be inaccurate\.
+ Hostname information may not be immediately available to the algorithm container\. We recommend adding a retry policy on hostname resolution operations as nodes become available in the cluster\.

The following is an example file on node 1 in a three\-node cluster:

```
{
    "current_host": "algo-1",
    "hosts": ["algo-1","algo-2","algo-3"],
    "network_interface_name":"eth1"
}
```