# Using Your Own Training Algorithms<a name="your-algorithms-training-algo"></a>

This section explains how Amazon SageMaker interacts with a Docker container that runs your custom training algorithm\. Use this information to write training code and create a Docker image for your training algorithms\. 

**Topics**
+ [How Amazon SageMaker Runs Your Training Image](#your-algorithms-training-algo-dockerfile)
+ [How Amazon SageMaker Provides Training Information](#your-algorithms-training-algo-running-container)
+ [Signalling Algorithm Success and Failure](#your-algorithms-training-signal-success-failure)
+ [How Amazon SageMaker Processes Training Output](#your-algorithms-training-algo-envvariables)
+ [Next Step](#byota-next-step)

## How Amazon SageMaker Runs Your Training Image<a name="your-algorithms-training-algo-dockerfile"></a>

To configure a Docker container to run as an executable, use an `ENTRYPOINT` instruction in a Dockerfile\. Note the following: 
+ For model training, Amazon SageMaker runs the container as follows:

  ```
  docker run image train
  ```

   Amazon SageMaker overrides any default `CMD` statement in a container by specifying the `train` argument after the image name\. The `train` argument also overrides arguments that you provide using `CMD` in the Dockerfile\. 

   
+ Use the `exec` form of the `ENTRYPOINT` instruction: 

  ```
  ENTRYPOINT ["executable", "param1", "param2"]
  ```

  For example:

  ```
  ENTRYPOINT ["python", "k-means-algorithm.py"]
  ```

  The `exec` form of the `ENTRYPOINT` instruction starts the executable directly, not as a child of `/bin/sh`\. This enables it to receive signals like `SIGTERM` and `SIGKILL` from Amazon SageMaker APIs\. Note the following:

   
  + The [CreateTrainingJob](API_CreateTrainingJob.md) API has a stopping condition that directs Amazon SageMaker to stop model training after a specific time\. 

     
  + The [StopTrainingJob](API_StopTrainingJob.md) API issues the equivalent of the `docker stop`, with a 2 minute timeout, command to gracefully stop the specified container:

    ```
    docker stop -t120
    ```

    The command attempts to stop the running container by sending a `SIGTERM` signal\. After the 2 minute timeout, SIGKILL is sent and the containers are forcibly stopped\. If the container handles the SIGTERM gracefully and exits within 120 seconds from receiving it, no SIGKILL is sent\. 
**Note**  
If you want access to the intermediate model artifacts after Amazon SageMaker stops the training, add code to handle saving artifacts in your `SIGTERM` handler\.
+ If you plan to use GPU devices for model training, make sure that your containers are `nvidia-docker` compatible\. Only the CUDA toolkit should be included on containers; don't bundle NVIDIA drivers with the image\. For more information about `nvidia-docker`, see [NVIDIA/nvidia\-docker](https://github.com/NVIDIA/nvidia-docker)\.
+ You can't use the `tini` initializer as your entry point in Amazon SageMaker containers because it gets confused by the train and serve arguments\.
+ `/opt/ml` and all sub\-directories are reserved by Amazon SageMaker training\. When building your algorithm’s docker image, please ensure you don't place any data required by your algorithm under them as the data may no longer be visible during training\.

## How Amazon SageMaker Provides Training Information<a name="your-algorithms-training-algo-running-container"></a>

This section explains how Amazon SageMaker makes training information, such as training data, hyperparameters, and other configuration information, available to your Docker container\. 

When you send a [CreateTrainingJob](API_CreateTrainingJob.md) request to Amazon SageMaker to start model training, you specify the Amazon Elastic Container Registry path of the Docker image that contain the training algorithm\. You also specify the Amazon Simple Storage Service \(Amazon S3\) location where training data is stored and algorithm\-specific parameters\. Amazon SageMaker makes this information available to the Docker container so that your training algorithm can use it\. This section explains how we make this information available to your Docker container\. For information about creating a training job, see `CreateTrainingJob`\.

**Topics**
+ [Hyperparameters](#your-algorithms-training-algo-running-container-hyperparameters)
+ [Environment Variables](#your-algorithms-training-algo-running-container-environment-variables)
+ [Input Data Configuration](#your-algorithms-training-algo-running-container-inputdataconfig)
+ [Training Data](#your-algorithms-training-algo-running-container-trainingdata)
+ [Distributed Training Configuration](#your-algorithms-training-algo-running-container-dist-training)

### Hyperparameters<a name="your-algorithms-training-algo-running-container-hyperparameters"></a>

 Amazon SageMaker makes the hyperparameters in a `CreateTrainingJob` request available in the Docker container in the `/opt/ml/input/config/hyperparameters.json` file\.

### Environment Variables<a name="your-algorithms-training-algo-running-container-environment-variables"></a>
+ TRAINING\_JOB\_NAME—The training job name stored in the `TrainingJobName` parameter in a [CreateTrainingJob](API_CreateTrainingJob.md) request\.

### Input Data Configuration<a name="your-algorithms-training-algo-running-container-inputdataconfig"></a>

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

### Training Data<a name="your-algorithms-training-algo-running-container-trainingdata"></a>

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

### Distributed Training Configuration<a name="your-algorithms-training-algo-running-container-dist-training"></a>

If you're performing distributed training with multiple containers, Amazon SageMaker makes information about all containers available in the `/opt/ml/input/config/resourceconfig.json` file\.

To enable inter\-container communication, this JSON file contains information for all containers\. Amazon SageMaker makes this file available for both FILE and PIPE mode algorithms\. The file provides the following information:
+ `current_host`—The name of the current container on the container network\. For example, `algo-1`\. Host values can change at any time\. Don't write code with specific values for this variable\.
+ `hosts`—The list of names of all containers on the container network, sorted lexicographically\. For example, `["algo-1", "algo-2", "algo-3"]` for a three\-node cluster\. Containers can use these names to address other containers on the container network\. Host values can change at any time\. Don't write code with specific values for these variables\.
+ Do not use the information in `/etc/hostname` or `/etc/hosts` because it might be inaccurate\.
+ Hostname information may not be immediately available to the algorithm container\. We recommend adding a retry policy on hostname resolution operations as nodes become available in the cluster\.

The following is an example file on node 1 in a three\-node cluster:

```
{
"current_host": "algo-1",
"hosts": ["algo-1","algo-2","algo-3"]
}
```

## Signalling Algorithm Success and Failure<a name="your-algorithms-training-signal-success-failure"></a>

A training algorithm indicates whether it succeeded or failed using the exit code of its process\. 

A successful training execution should exit with an exit code of 0 and an unsuccessful training execution should exit with a non\-zero exit code\. These will be converted to "Completed" and "Failed" in the `TrainingJobStatus` returned by `DescribeTrainingJob`\. This exit code convention is standard and is easily implemented in all languages\. For example, in Python, you can use `sys.exit(1)` to signal a failure exit and simply running to the end of the main routine will cause Python to exit with code 0\.

In the case of failure, the algorithm can write a description of the failure to the failure file\. See next section for details\.

## How Amazon SageMaker Processes Training Output<a name="your-algorithms-training-algo-envvariables"></a>

As your algorithm runs in a container, it generates output including the status of the training job and model and output artifacts\. Your algorithm should write this information to the following files, which are located in the container's `/output` directory\. Amazon SageMaker processes the information contained in this directory as follows:
+ `/opt/ml/output/failure`—If training fails, after all algorithm output \(for example, logging\) completes, your algorithm should write the failure description to this file\. In a `DescribeTrainingJob` response, Amazon SageMaker returns the first 1024 characters from this file as `FailureReason`\. 

   
+ `/opt/ml/model`—Your algorithm should write all final model artifacts to this directory\. Amazon SageMaker copies this data as a single object in compressed tar format to the S3 location that you specified in the `CreateTrainingJob` request\.  If multiple containers in a single training job write to this directory they should ensure no `file/directory` names clash\. Amazon SageMaker aggregates the result in a tar file and uploads to s3\. 

## Next Step<a name="byota-next-step"></a>

 [Using Your Own Inference Code](your-algorithms-inference-main.md) 