# Using Your Own Inference Code<a name="your-algorithms-inference-code"></a>

This section explains how Amazon SageMaker interacts with a Docker container that runs your own inference code\. Use this information to write inference code and create a Docker image\. 

**Topics**
+ [How Amazon SageMaker Runs Your Inference Image](#your-algorithms-inference-code-run-image)
+ [How Amazon SageMaker Loads Your Model Artifacts](#your-algorithms-inference-code-load-artifacts)
+ [How Containers Serve Requests](#your-algorithms-inference-code-how-containe-serves-requests)
+ [How Your Container Should Respond to Inference Requests](#your-algorithms-inference-code-container-response)
+ [How Your Container Should Respond to Health Check \(Ping\) Requests](#your-algorithms-inference-algo-ping-requests)

## How Amazon SageMaker Runs Your Inference Image<a name="your-algorithms-inference-code-run-image"></a>

To configure a container to run as an executable, use an `ENTRYPOINT` instruction in a Dockerfile\. Note the following: 
+ For model inference, Amazon SageMaker runs the container as:

  ```
  docker run image serve
  ```

  Amazon SageMaker overrides default `CMD` statements in a container by specifying the `serve` argument after the image name\. The `serve` argument overrides arguments that you provide with the `CMD` command in the Dockerfile\.

   
+ We recommend that you use the `exec` form of the `ENTRYPOINT` instruction:

  ```
  ENTRYPOINT ["executable", "param1", "param2"]
  ```

  For example:

  ```
  ENTRYPOINT ["python", "k_means_inference.py"]
  ```

  The `exec` form of the `ENTRYPOINT` instruction starts the executable directly, not as a child of `/bin/sh`\. This enables it to receive signals like `SIGTERM` and `SIGKILL` from the Amazon SageMaker APIs, which is a requirement\. 

   

  For example, when you use the [CreateEndpoint](API_CreateEndpoint.md) API to create an endpoint, Amazon SageMaker provisions the number of ML compute instances required by the endpoint configuration, which you specify in the request\. Amazon SageMaker runs the Docker container on those instances\. 

   

  If you reduce the number of instances backing the endpoint \(by calling the [UpdateEndpointWeightsAndCapacities](API_UpdateEndpointWeightsAndCapacities.md) APIs\), Amazon SageMaker runs a command to stop the Docker container on the instances being terminated\. The command sends the `SIGTERM` signal, then it sends the `SIGKILL` signal thirty seconds later\.

   

  If you update the endpoint \(by calling the [UpdateEndpoint](API_UpdateEndpoint.md) API\), Amazon SageMaker launches another set of ML compute instances and runs the Docker containers that contain your inference code on them\. Then it runs a command to stop the previous Docker container\. To stop the Docker container, command sends the `SIGTERM` signal, then it sends the `SIGKILL` signal thirty seconds later\. 

   
+ Amazon SageMaker uses the container definition that you provided in your [CreateModel](API_CreateModel.md) request to set environment variables and the DNS hostname for the container as follows:

   
  + It sets environment variables using the `ContainerDefintion.Environment` string\-to\-string map\.
  + It sets the DNS hostname using the `ContainerDefinition.ContainerHostname`\.

     
+ If you plan to use GPU devices for model inferences \(by specifying GPU\-based ML compute instances in your `CreateEndpointConfig` request\), make sure that your containers are `nvidia-docker` compatible\. Don't bundle NVIDIA drivers with the image\. For more information about `nvidia-docker`, see [NVIDIA/nvidia\-docker](https://github.com/NVIDIA/nvidia-docker)\. 

   
+ You can't use the `tini` initializer as your entry point in Amazon SageMaker containers because it gets confused by the train and serve arguments\.

## How Amazon SageMaker Loads Your Model Artifacts<a name="your-algorithms-inference-code-load-artifacts"></a>

In your [CreateModel](API_CreateModel.md) request, the container definition includes the `ModelDataUrl` parameter, which identifies the S3 location where model artifacts are stored\. Amazon SageMaker uses this information to determine where to copy the model artifacts from\. It copies the artifacts to the `/opt/ml/model` directory for use by your inference code\.

The `ModelDataUrl` must point to a tar\.gz file, anything else will result in failure to download the file\. 

 Amazon SageMaker stores the model artifact as a single compressed tar file in Amazon S3\. Amazon SageMaker uncompresses this tar file into the `/opt/ml/model` directory before your container starts\. If you used Amazon SageMaker to train the model, the files will appear just as you left them\.

## How Containers Serve Requests<a name="your-algorithms-inference-code-how-containe-serves-requests"></a>

Containers need to implement a web server that responds to `/invocations` and `/ping` on port 8080\. 

## How Your Container Should Respond to Inference Requests<a name="your-algorithms-inference-code-container-response"></a>

To obtain inferences, the client application sends a POST request to the Amazon SageMaker endpoint\. For more information, see the [InvokeEndpoint](API_runtime_InvokeEndpoint.md) API\. Amazon SageMaker passes the request to the container, and returns the inference result from the container to the client\. Note the following:
+ Amazon SageMaker strips all `POST` headers except those supported by `InvokeEndpoint`\. Amazon SageMaker might add additional headers\. Inference containers must be able to safely ignore these additional headers\.
+ To receive inference requests, the container must have a web server listening on port 8080 and must accept `POST` requests to the `/invocations` endpoint\. 

## How Your Container Should Respond to Health Check \(Ping\) Requests<a name="your-algorithms-inference-algo-ping-requests"></a>

The `CreateEndpoint` and `UpdateEndpoint` API calls result in Amazon SageMaker starting new inference containers\. Soon after container startup, Amazon SageMaker starts sending periodic GET requests to the /ping endpoint\.

The simplest requirement on the container is to respond with an HTTP 200 status code and an empty body\. This indicates to Amazon SageMaker that the container is ready to accept inference requests at the /invocations endpoint\.

If the container does not begin to consistently respond with 200s during the first 30 seconds after startup, the `CreateEndPoint` and `UpdateEndpoint` APIs will fail\.

While the minimum bar is for the container to return a static 200, a container developer can use this functionality to perform deeper checks\. The request timeout on /ping attempts is 2 seconds\.