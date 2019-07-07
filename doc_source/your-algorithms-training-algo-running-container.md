# How Amazon SageMaker Provides Training Information<a name="your-algorithms-training-algo-running-container"></a>

This section explains how Amazon SageMaker makes training information, such as training data, hyperparameters, and other configuration information, available to your Docker container\. 

When you send a [CreateTrainingJob](API_CreateTrainingJob.md) request to Amazon SageMaker to start model training, you specify the Amazon Elastic Container Registry path of the Docker image that contains the training algorithm\. You also specify the Amazon Simple Storage Service \(Amazon S3\) location where training data is stored and algorithm\-specific parameters\. Amazon SageMaker makes this information available to the Docker container so that your training algorithm can use it\. This section explains how we make this information available to your Docker container\. For information about creating a training job, see `CreateTrainingJob`\.

**Topics**
+ [Hyperparameters](#your-algorithms-training-algo-running-container-hyperparameters)
+ [Environment Variables](#your-algorithms-training-algo-running-container-environment-variables)
+ [Input Data Configuration](#your-algorithms-training-algo-running-container-inputdataconfig)
+ [Training Data](#your-algorithms-training-algo-running-container-trainingdata)
+ [Distributed Training Configuration](#your-algorithms-training-algo-running-container-dist-training)

## Hyperparameters<a name="your-algorithms-training-algo-running-container-hyperparameters"></a>

 Amazon SageMaker makes the hyperparameters in a `CreateTrainingJob` request available in the Docker container in the `/opt/ml/input/config/hyperparameters.json` file\.

## Environment Variables<a name="your-algorithms-training-algo-running-container-environment-variables"></a>
+ TRAINING\_JOB\_NAME—The training job name stored in the `TrainingJobName` parameter in a [CreateTrainingJob](API_CreateTrainingJob.md) request\.

## Input Data Configuration<a name="your-algorithms-training-algo-running-container-inputdataconfig"></a>

You specify data channel information in the `InputDataConfig` parameter in a `CreateTrainingJob` request\. Amazon SageMaker makes this information available in the `/opt/ml/input/config/inputdataconfig.json` file in the Docker container\.

For example, suppose that you specify three data channels \(`train`, `evaluation`, and `validation`\) in your request\. Amazon SageMaker provides the following JSON:

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
Amazon SageMaker provides only relevant information about each data channel \(for example, the channel name and the content type\) to the container, as shown\. 

## Training Data<a name="your-algorithms-training-algo-running-container-trainingdata"></a>

The `TrainingInputMode` parameter in a `CreateTrainingJob` request specifies how to make data available for model training: in `FILE` mode or `PIPE` mode\. Depending on the specified input mode, Amazon SageMaker does the following:
+  `FILE` mode—Amazon SageMaker makes the data for the channel available in the `/opt/ml/input/data/channel_name` directory in the Docker container\. For example, if you have three channels named `training`, `validation`, and `testing`, Amazon SageMaker makes three directories in the Docker container: 
  + `/opt/ml/input/data/training`
  + `/opt/ml/input/data/validation`
  + `/opt/ml/input/data/testing`

     
+  `PIPE` mode—Amazon SageMaker makes data for the channel available from the named pipe: `/opt/ml/input/data/channel_name_epoch_number`\. For example, if you have three channels named `training`, `validation`, and `testing`, you will need to read from the following pipes:
  + `/opt/ml/input/data/training_0,/opt/ml/input/data/training_1, ...`
  + `/opt/ml/input/data/validation_0, /opt/ml/input/data/validation_1, ...`
  + `/opt/ml/input/data/testing_0, /opt/ml/input/data/testing_1, ...`

  Read the pipes sequentially\. For example, if you have a channel called **training**, read the pipes in this sequence: 

  1. Open `/opt/ml/input/data/training_0` in read mode and read it to EOF \(or if you are done with the first epoch, close the file early\)\. 

  1. After closing the first pipe file, look for `/opt/ml/input/data/training_1` and read it to go through the second epoch, and so on\.

  If the file for a given epoch doesn't exist yet, your code may need to retry until the pipe is created\. There is no sequencing restriction across channel types\. That is, you can read multiple epochs for the **training** channel, for example, and only start reading the **validation** channel when you are ready\. Or, you can read them simultaneously if your algorithm requires that\. 

## Distributed Training Configuration<a name="your-algorithms-training-algo-running-container-dist-training"></a>

If you're performing distributed training with multiple containers, Amazon SageMaker makes information about all containers available in the `/opt/ml/input/config/resourceconfig.json` file\.

To enable inter\-container communication, this JSON file contains information for all containers\. Amazon SageMaker makes this file available for both FILE and PIPE mode algorithms\. The file provides the following information:
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