# Using Your Own Inference Code \(Batch Transform\)<a name="your-algorithms-batch-code"></a>

This section explains how Amazon SageMaker interacts with a Docker container that runs your own inference code for batch transform\. Use this information to write inference code and create a Docker image\. 

**Topics**
+ [How Amazon SageMaker Runs Your Inference Image in Batch Transform](#your-algorithms-batch-code-run-image)
+ [How Amazon SageMaker Loads Your Model Artifacts](#your-algorithms-batch-code-load-artifacts)
+ [How Containers Serve Requests](#your-algorithms-batch-code-how-containe-serves-requests)
+ [How Your Container Should Respond to Health Check \(Ping\) Requests](#your-algorithms-batch-algo-ping-requests)

## How Amazon SageMaker Runs Your Inference Image in Batch Transform<a name="your-algorithms-batch-code-run-image"></a>

To configure a container to run as an executable, use an `ENTRYPOINT` instruction in a Dockerfile\. Note the following: 
+ For batch transform, Amazon SageMaker runs the container as:

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

   
+ Amazon SageMaker sets environment variables specified in [CreateModel](API_CreateModel.md) and [CreateTransformJob](API_CreateTransformJob.md) on your container\. Additionally, the following environment variables will be populated:

   
  + `SAGEMAKER_BATCH` is always set to `true` when the container runs in Batch Transform\.
  + `SAGEMAKER_MAX_PAYLOAD_IN_MB` is set to the largest size payload that will be sent to the container via HTTP\.
  + `SAGEMAKER_BATCH_STRATEGY` will be set to `SINGLE_RECORD` when the container will be sent a single record per call to invocations and `MULTI_RECORD` when the container will get as many records as will fit in the payload\.
  + `SAGEMAKER_MAX_CONCURRENT_TRANSFORMS` is set to the maximum number of `/invocations` requests that can be opened simultaneously\.
**Note**  
The last three environment variables come from the API call made by the user\. If the user doesn’t set values to them then they will not be passed\. At which case, either the default values or the values requested by the algorithm \(in response to the `/execution-parameters`\) are used\.  
 
+ If you plan to use GPU devices for model inferences \(by specifying GPU\-based ML compute instances in your `CreateTransformJob` request\), make sure that your containers are nvidia\-docker compatible\. Don't bundle NVIDIA drivers with the image\. For more information about nvidia\-docker, see [NVIDIA/nvidia\-docker](https://github.com/NVIDIA/nvidia-docker)\. 

   
+ You can't use the `tini` initializer as your entry point in Amazon SageMaker containers because it gets confused by the train and serve arguments\.

## How Amazon SageMaker Loads Your Model Artifacts<a name="your-algorithms-batch-code-load-artifacts"></a>

In your [CreateModel](API_CreateModel.md) request, the container definition includes the `ModelDataUrl` parameter, which identifies the S3 location where model artifacts are stored\. Amazon SageMaker uses this information to determine where to copy the model artifacts from\. It copies the artifacts to the `/opt/ml/model` directory for use by your inference code\.

The `ModelDataUrl` must point to a tar\.gz file, or else it will fail to download the file\. 

 Amazon SageMaker stores the model artifacts as a single compressed tar file in Amazon S3\. Amazon SageMaker decompresses this tar file into the `/opt/ml/model` directory before your container starts\. If you used Amazon SageMaker to train a model, the tar file will be stored in S3\.

## How Containers Serve Requests<a name="your-algorithms-batch-code-how-containe-serves-requests"></a>

Containers need to implement a web server that responds to `/invocations` and `/ping` on port 8080\. 

Containers can optionally provide dynamic information to Amazon SageMaker for better throughput\. If you want to provide this information dynamically, implement `/execution-parameters` on port 8080 in addition to `/invocations` and `/ping`\. The response for `GET /execution-parameters` should be a simple JSON object with keys for `MaxConcurrentTransforms`, `BatchStrategy`, and `MaxPayloadInMB`\. Here is an example valid response: 

```
{
  “MaxConcurrentTransforms”: 8,
  “BatchStrategy": "MULTI_RECORD",
  "MaxPayloadInMB": 6
}
```

Amazon SageMaker will attempt invoking `/execution-parameters` before calling `/invocations`\. If the container does not implement `/execution-parameters`, Amazon SageMaker applies these defaults:


| Parameter | Possible Values | Default Values | Additional Notes | 
| --- | --- | --- | --- | 
|  MaxConcurrentTransforms  |  1\-100  |  1  |   | 
|  BatchStrategy  |  `SINGLE_RECORD`, `MULTI_RECORD`  |  `MULTI_RECORD`  |   | 
|  MaxPayloadInMB  |  ≧0  |  6  |  0 signifies that the payload can be arbitrarily large and is transmitted in HTTP chunked encoding\.  | 

## How Your Container Should Respond to Health Check \(Ping\) Requests<a name="your-algorithms-batch-algo-ping-requests"></a>

The simplest requirement on the container is to respond with an HTTP 200 status code and an empty body\. This indicates to Amazon SageMaker that the container is ready to accept inference requests at the `/invocations` endpoint\.

While the minimum bar is for the container to return a static 200, a container developer can use this functionality to perform deeper checks\. The request timeout on `/ping` attempts is 2 seconds\.