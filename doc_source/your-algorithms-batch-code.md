# Use Your Own Inference Code with Batch Transform<a name="your-algorithms-batch-code"></a>

This section explains how Amazon SageMaker interacts with a Docker container that runs your own inference code for batch transform\. Use this information to write inference code and create a Docker image\. 

**Topics**
+ [How SageMaker Runs Your Inference Image](#your-algorithms-batch-code-run-image)
+ [How SageMaker Loads Your Model Artifacts](#your-algorithms-batch-code-load-artifacts)
+ [How Containers Serve Requests](#your-algorithms-batch-code-how-containe-serves-requests)
+ [How Your Container Should Respond to Inference Requests](#your-algorithms-batch-code-how-containers-should-respond-to-inferences)
+ [How Your Container Should Respond to Health Check \(Ping\) Requests](#your-algorithms-batch-algo-ping-requests)

## How SageMaker Runs Your Inference Image<a name="your-algorithms-batch-code-run-image"></a>

To configure a container to run as an executable, use an `ENTRYPOINT` instruction in a Dockerfile\. Note the following: 
+ For batch transforms, SageMaker runs the container as:

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

   
+ SageMaker sets environment variables specified in [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) and [ `CreateTransformJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html) on your container\. Additionally, the following environment variables will be populated:
  + `SAGEMAKER_BATCH` is always set to `true` when the container runs in Batch Transform\.
  + `SAGEMAKER_MAX_PAYLOAD_IN_MB` is set to the largest size payload that will be sent to the container via HTTP\.
  + `SAGEMAKER_BATCH_STRATEGY` will be set to `SINGLE_RECORD` when the container will be sent a single record per call to invocations and `MULTI_RECORD` when the container will get as many records as will fit in the payload\.
  + `SAGEMAKER_MAX_CONCURRENT_TRANSFORMS` is set to the maximum number of `/invocations` requests that can be opened simultaneously\.
**Note**  
The last three environment variables come from the API call made by the user\. If the user doesn’t set values for them, they aren't passed\. In that case, either the default values or the values requested by the algorithm \(in response to the `/execution-parameters`\) are used\.
+ If you plan to use GPU devices for model inferences \(by specifying GPU\-based ML compute instances in your `CreateTransformJob` request\), make sure that your containers are nvidia\-docker compatible\. Don't bundle NVIDIA drivers with the image\. For more information about nvidia\-docker, see [NVIDIA/nvidia\-docker](https://github.com/NVIDIA/nvidia-docker)\. 

   
+ You can't use the `init` initializer as your entry point in SageMaker containers because it gets confused by the train and serve arguments\.

## How SageMaker Loads Your Model Artifacts<a name="your-algorithms-batch-code-load-artifacts"></a>

In a [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) request, container definitions includes the `ModelDataUrl` parameter, which identifies the location in Amazon S3 where model artifacts are stored\. When you use SageMaker to run inferences, it uses this information to determine where to copy the model artifacts from\. It copies the artifacts to the `/opt/ml/model` directory in the Docker container for use by your inference code\.

The `ModelDataUrl` parameter must point to a tar\.gz file\. Otherwise, SageMaker can't download the file\. If you train a model in SageMaker, it saves the artifacts as a single compressed tar file in Amazon S3\. If you train a model in another framework, you need to store the model artifacts in Amazon S3 as a compressed tar file\. SageMaker decompresses this tar file and saves it in the `/opt/ml/model` directory in the container before the batch transform job starts\. 

## How Containers Serve Requests<a name="your-algorithms-batch-code-how-containe-serves-requests"></a>

Containers must implement a web server that responds to invocations and ping requests on port 8080\. For batch transforms, you have the option to set algorithms to implement execution\-parameters requests to provide a dynamic runtime configuration to SageMaker\. SageMaker uses the following endpoints: 
+ `ping`—Used to periodically check the health of the container\. SageMaker waits for an HTTP `200` status code and an empty body for a successful ping request before sending an invocations request\. You might use a ping request to load a model into memory to generate inference when invocations requests are sent\.
+ \(Optional\) `execution-parameters`—Allows the algorithm to provide the optimal tuning parameters for a job during runtime\. Based on the memory and CPUs available for a container, the algorithm chooses the appropriate `MaxConcurrentTransforms`, `BatchStrategy`, and `MaxPayloadInMB` values for the job\.

Before calling the invocations request, SageMaker attempts to invoke the execution\-parameters request\. When you create a batch transform job, you can provide values for the `MaxConcurrentTransforms`, `BatchStrategy`, and `MaxPayloadInMB` parameters\. SageMaker determines the values for these parameters using this order of precedence:

1. The parameter values that you provide when you create the `CreateTransformJob` request,

1. The values that the model container returns when SageMaker invokes the execution\-parameters endpoint

1. The parameters default values, listed in the following table\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-batch-code.html)

The response for a `GET` execution\-parameters request is a JSON object with keys for `MaxConcurrentTransforms`, `BatchStrategy`, and `MaxPayloadInMB` parameters\. This is an example of a valid response:

```
{
“MaxConcurrentTransforms”: 8,
“BatchStrategy": "MULTI_RECORD",
"MaxPayloadInMB": 6
}
```

## How Your Container Should Respond to Inference Requests<a name="your-algorithms-batch-code-how-containers-should-respond-to-inferences"></a>

To obtain inferences, Amazon SageMaker sends a POST request to the inference container\. The POST request body contains data from S3\. Amazon SageMaker passes the request to the container, and returns the inference result from the container, saving the data from the response to S3\.

To receive inference requests, the container must have a web server listening on port 8080 and must accept POST requests to the `/invocations` endpoint\. Your model containers must respond to requests within 600 seconds\.

## How Your Container Should Respond to Health Check \(Ping\) Requests<a name="your-algorithms-batch-algo-ping-requests"></a>

The simplest requirement on the container is to respond with an HTTP 200 status code and an empty body\. This indicates to SageMaker that the container is ready to accept inference requests at the `/invocations` endpoint\.

While the minimum bar is for the container to return a static 200, a container developer can use this functionality to perform deeper checks\. The request timeout on `/ping` attempts is 2 seconds\.