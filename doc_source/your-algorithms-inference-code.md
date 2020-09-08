# Use Your Own Inference Code with Hosting Services<a name="your-algorithms-inference-code"></a>

This section explains how Amazon SageMaker interacts with a Docker container that runs your own inference code for hosting services\. Use this information to write inference code and create a Docker image\. 

**Topics**
+ [How SageMaker Runs Your Inference Image](#your-algorithms-inference-code-run-image)
+ [How SageMaker Loads Your Model Artifacts](#your-algorithms-inference-code-load-artifacts)
+ [How Containers Serve Requests](#your-algorithms-inference-code-how-containe-serves-requests)
+ [How Your Container Should Respond to Inference Requests](#your-algorithms-inference-code-container-response)
+ [How Your Container Should Respond to Health Check \(Ping\) Requests](#your-algorithms-inference-algo-ping-requests)
+ [Use a Private Docker Registry for Real\-Time Inference Containers](your-algorithms-containers-inference-private.md)

## How SageMaker Runs Your Inference Image<a name="your-algorithms-inference-code-run-image"></a>

To configure a container to run as an executable, use an `ENTRYPOINT` instruction in a Dockerfile\. Note the following: 
+ For model inference, SageMaker runs the container as:

  ```
  docker run image serve
  ```

  SageMaker overrides default `CMD` statements in a container by specifying the `serve` argument after the image name\. The `serve` argument overrides arguments that you provide with the `CMD` command in the Dockerfile\.

   
+ We recommend that you use the `exec` form of the `ENTRYPOINT` instruction:

  ```
  ENTRYPOINT ["executable", "param1", "param2"]
  ```

  For example:

  ```
  ENTRYPOINT ["python", "k_means_inference.py"]
  ```

  The `exec` form of the `ENTRYPOINT` instruction starts the executable directly, not as a child of `/bin/sh`\. This enables it to receive signals like `SIGTERM` and `SIGKILL` from the SageMaker APIs, which is a requirement\. 

   

  For example, when you use the [ `CreateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API to create an endpoint, SageMaker provisions the number of ML compute instances required by the endpoint configuration, which you specify in the request\. SageMaker runs the Docker container on those instances\. 

   

  If you reduce the number of instances backing the endpoint \(by calling the [ `UpdateEndpointWeightsAndCapacities`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpointWeightsAndCapacities.html) APIs\), SageMaker runs a command to stop the Docker container on the instances being terminated\. The command sends the `SIGTERM` signal, then it sends the `SIGKILL` signal thirty seconds later\.

   

  If you update the endpoint \(by calling the [ `UpdateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API\), SageMaker launches another set of ML compute instances and runs the Docker containers that contain your inference code on them\. Then it runs a command to stop the previous Docker containers\. To stop a Docker container, command sends the `SIGTERM` signal, then it sends the `SIGKILL` signal thirty seconds later\. 

   
+ SageMaker uses the container definition that you provided in your [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) request to set environment variables and the DNS hostname for the container as follows:

   
  + It sets environment variables using the `ContainerDefinition.Environment` string\-to\-string map\.
  + It sets the DNS hostname using the `ContainerDefinition.ContainerHostname`\.

     
+ If you plan to use GPU devices for model inferences \(by specifying GPU\-based ML compute instances in your `CreateEndpointConfig` request\), make sure that your containers are `nvidia-docker` compatible\. Don't bundle NVIDIA drivers with the image\. For more information about `nvidia-docker`, see [NVIDIA/nvidia\-docker](https://github.com/NVIDIA/nvidia-docker)\. 

   
+ You can't use the `tini` initializer as your entry point in SageMaker containers because it gets confused by the train and serve arguments\.

## How SageMaker Loads Your Model Artifacts<a name="your-algorithms-inference-code-load-artifacts"></a>

In your [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) request, the container definition includes the `ModelDataUrl` parameter, which identifies the S3 location where model artifacts are stored\. SageMaker uses this information to determine where to copy the model artifacts from\. It copies the artifacts to the `/opt/ml/model` directory for use by your inference code\.

The `ModelDataUrl` must point to a tar\.gz file\. Otherwise, SageMaker won't download the file\. 

If you trained your model in SageMaker, the model artifacts are saved as a single compressed tar file in Amazon S3\. If you trained your model outside SageMaker, you need to create this single compressed tar file and save it in a S3 location\. SageMaker decompresses this tar file into /opt/ml/model directory before your container starts\.

## How Containers Serve Requests<a name="your-algorithms-inference-code-how-containe-serves-requests"></a>

Containers need to implement a web server that responds to `/invocations` and `/ping` on port 8080\. 

## How Your Container Should Respond to Inference Requests<a name="your-algorithms-inference-code-container-response"></a>

To obtain inferences, the client application sends a POST request to the SageMaker endpoint\. For more information, see the [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html) API\. SageMaker passes the request to the container, and returns the inference result from the container to the client\. Note the following:
+ SageMaker strips all `POST` headers except those supported by `InvokeEndpoint`\. SageMaker might add additional headers\. Inference containers must be able to safely ignore these additional headers\.
+ To receive inference requests, the container must have a web server listening on port 8080 and must accept `POST` requests to the `/invocations` endpoint\. 
+ A customer's model containers must accept socket connection requests within 250 ms\.
+ A customer's model containers must respond to requests within 60 seconds\. The model itself can have a maximum processing time of 60 seconds before responding to the /invocations\. If your model is going to take 50\-60 seconds of processing time, the SDK socket timeout should be set to be 70 seconds\.

## How Your Container Should Respond to Health Check \(Ping\) Requests<a name="your-algorithms-inference-algo-ping-requests"></a>

The `CreateEndpoint` and `UpdateEndpoint` API calls result in SageMaker starting new inference containers\. Soon after container startup, SageMaker starts sending periodic GET requests to the /ping endpoint\.

The simplest requirement on the container is to respond with an HTTP 200 status code and an empty body\. This indicates to SageMaker that the container is ready to accept inference requests at the /invocations endpoint\.

If the container does not begin to pass health checks, by consistently responding with 200s, during the 4 minutes after startup, `CreateEndPoint` will fail, leaving Endpoint in a failed state, and the update requested by `UpdateEndpoint` will not be completed\.

While the minimum bar is for the container to return a static 200, a container developer can use this functionality to perform deeper checks\. The request timeout on /ping attempts is 2 seconds\.