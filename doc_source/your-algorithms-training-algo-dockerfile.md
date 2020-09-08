# How Amazon SageMaker Runs Your Training Image<a name="your-algorithms-training-algo-dockerfile"></a>

To configure a Docker container to run as an executable, use an `ENTRYPOINT` instruction in a Dockerfile\. Note the following: 
+ For model training, Amazon SageMaker runs the container as follows:

  ```
  docker run image train
  ```

   SageMaker overrides any default `CMD` statement in a container by specifying the `train` argument after the image name\. The `train` argument also overrides arguments that you provide using `CMD` in the Dockerfile\. 

   
+ In your dockerfile, use the `exec` form of the `ENTRYPOINT` instruction: 

  ```
  ENTRYPOINT ["executable", "param1", "param2", ...]
  ```

  For example:

  ```
  ENTRYPOINT ["python", "k-means-algorithm.py"]
  ```

  The `exec` form of the `ENTRYPOINT` instruction starts the executable directly, not as a child of `/bin/sh`\. This enables it to receive signals like `SIGTERM` and `SIGKILL` from SageMaker APIs\. Note the following:

   
  + The [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API has a stopping condition that directs SageMaker to stop model training after a specific time\. 

     
  + The [ `StopTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopTrainingJob.html) API issues the equivalent of the `docker stop`, with a 2 minute timeout, command to gracefully stop the specified container:

    ```
    docker stop -t120
    ```

    The command attempts to stop the running container by sending a `SIGTERM` signal\. After the 2 minute timeout, SIGKILL is sent and the containers are forcibly stopped\. If the container handles the SIGTERM gracefully and exits within 120 seconds from receiving it, no SIGKILL is sent\. 
**Note**  
If you want access to the intermediate model artifacts after SageMaker stops the training, add code to handle saving artifacts in your `SIGTERM` handler\.
+ If you plan to use GPU devices for model training, make sure that your containers are `nvidia-docker` compatible\. Only the CUDA toolkit should be included on containers; don't bundle NVIDIA drivers with the image\. For more information about `nvidia-docker`, see [NVIDIA/nvidia\-docker](https://github.com/NVIDIA/nvidia-docker)\.
+ You can't use the `tini` initializer as your entry point in SageMaker containers because it gets confused by the train and serve arguments\.
+ `/opt/ml` and all sub\-directories are reserved by SageMaker training\. When building your algorithm’s docker image, please ensure you don't place any data required by your algorithm under them as the data may no longer be visible during training\.