# Build Your Own Container with Multi Model Server<a name="build-multi-model-build-container"></a>

Custom Elastic Container Registry \(ECR\) images deployed in Amazon SageMaker are expected to adhere to the basic contract described in [Use Your Own Inference Code with Hosting Services](your-algorithms-inference-code.md) that govern how Amazon SageMaker interacts with a Docker container that runs your own inference code\. For a container to be capable of loading and serving multiple models concurrently, there are additional APIs and behaviors that must be followed\. This additional contract includes new APIs to load, list, get, and unload models, and a different API to invoke models\. There are also different behaviors for error scenarios that the APIs need to abide by\. To indicate that the container complies with the additional requirements, you can add the following command to your Docker file:

```
LABEL com.amazonaws.sagemaker.capabilities.multi-models=true
```

Amazon SageMaker also injects an environment variable into the container

```
SAGEMAKER_MULTI_MODEL=true
```

To help you implement these requirements for a custom container, two libraries are available:
+ [Multi Model Server](https://github.com/awslabs/multi-model-server) is an open source framework for serving machine learning models that can be installed in containers to provide the front end that fulfills the requirements for the new multi\-model endpoint container APIs\. It provides the HTTP front end and model management capabilities required by multi\-model endpoints to host multiple models within a single container, load models into and unload models out of the container dynamically, and performs inference on a specified loaded model\. It also provides a pluggable backend that supports a pluggable custom backend handler where you can implement your own algorithm\.
+ [Amazon SageMaker Inference Toolkit](https://github.com/aws/sagemaker-inference-toolkit) is a library that bootstraps Multi Model Server with a configuration and settings that make it compatible with Amazon SageMaker multi\-model endpoints\. It also allows you to tweak important performance parameters, such as the number of workers per model, depending on the needs of your scenario\. 

For a sample notebook that shows how to set up and deploy a custom container that supports multi\-model endpoints in Amazon SageMaker, see the [Multi\-Model Endpoint BYOC Sample Notebook](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/multi_model_bring_your_own)\. 

## Contract for Custom Containers to Serve Multiple Model<a name="mms-container-apis"></a>

To handle multiple models, your container must support a set of APIs that enable the Amazon SageMaker platform to communicate with the container for loading, listing, getting, and unloading models as required\. The `model_name` is used in the new set of APIs as the key input parameter\. The customer container is expected to keep track of the loaded models using `model_name` as the mapping key\. Also, the `model_name` is an opaque identifier and is not necessarily the value of the `TargetModel` parameter passed into the `InvokeEndpoint` API\. The original `TargetModel` value in the `InvokeEndpoint` request is passed to container in the APIs as a `X-Amzn-Target-Model` header that can be used for logging purposes\.

**Topics**
+ [LOAD MODEL API](#multi-model-api-load-model)
+ [LIST MODEL API](#multi-model-api-list-model)
+ [GET MODEL API](#multi-model-api-get-model)
+ [UNLOAD MODEL API](#multi-model-api-unload-model)
+ [INVOKE MODEL API](#multi-model-api-invoke-model)

### LOAD MODEL API<a name="multi-model-api-load-model"></a>

Instructs the container to load a particular model present in the `url` field of the body into the memory of the customer container and to keep track of it with the assigned `model_name`\. After a model is loaded, the container should be ready to serve inference requests using this `model_name`\.

```
POST /models HTTP/1.1
Content-Type: application/json
Accept: application/json

{
     "model_name" : "{model_name}",
     "url" : "/opt/ml/models/{model_name}/model",
}
```

**Note**  
If `model_name` is already loaded, this API should return 409\. Any time a model cannot be loaded due to lack of memory or to any other resource, this API should return a 507 HTTP status code to Amazon SageMaker, which then initiates unloading unused models to reclaim\.

### LIST MODEL API<a name="multi-model-api-list-model"></a>

Returns the list of models loaded into the memory of the customer container\.

```
GET /models HTTP/1.1
Accept: application/json

Response = 
{
    "models": [
        {
             "modelName" : "{model_name}",
             "modelUrl" : "/opt/ml/models/{model_name}/model",
        },
        {
            "modelName" : "{model_name}",
            "modelUrl" : "/opt/ml/models/{model_name}/model",
        },
        ....
    ]
}
```

This API also supports pagination\.

```
GET /models HTTP/1.1
Accept: application/json

Response = 
{
    "models": [
        {
             "modelName" : "{model_name}",
             "modelUrl" : "/opt/ml/models/{model_name}/model",
        },
        {
            "modelName" : "{model_name}",
            "modelUrl" : "/opt/ml/models/{model_name}/model",
        },
        ....
    ]
}
```

Amazon SageMaker can initially call the List Models API without providing a value for `next_page_token`\. If a `nextPageToken` field is returned as part of the response, it will be provided as the value for `next_page_token` in a subsequent List Models call\. If a `nextPageToken` is not returned, it means that there are no more models to return\.

### GET MODEL API<a name="multi-model-api-get-model"></a>

This is a simple read API on the `model_name` entity\.

```
GET /models/{model_name} HTTP/1.1
Accept: application/json

{
     "modelName" : "{model_name}",
     "modelUrl" : "/opt/ml/models/{model_name}/model",
}
```

**Note**  
If `model_name` is not loaded, this API should return 404\.

### UNLOAD MODEL API<a name="multi-model-api-unload-model"></a>

Instructs the Amazon SageMaker platform to instruct the customer container to unload a model from memory\. This initiates the eviction of a candidate model as determined by the platform when starting the process of loading a new model\. The resources provisioned to `model_name` should be reclaimed by the container when this API returns a response\.

```
DELETE /models/{model_name}
```

**Note**  
If `model_name` is not loaded, this API should return 404\.

### INVOKE MODEL API<a name="multi-model-api-invoke-model"></a>

Makes a prediction request from the particular `model_name` supplied\. The Amazon SageMaker Runtime `InvokeEndpoint` request supports `X-Amzn-Target-Model` as a new header that takes the relative path of the model specified for invocation\. The Amazon SageMaker system constructs the absolute path of the model by combining the prefix that is provided as part of the `CreateModel` API call with the relative path of the model\.

```
POST /models/{model_name}/invoke HTTP/1.1
Content-Type: ContentType
Accept: Accept
X-Amzn-SageMaker-Custom-Attributes: CustomAttributes
X-Amzn-SageMaker-Target-Model: [relativePath]/{artifactName}.tar.gz
```

**Note**  
If `model_name` is not loaded, this API should return 404\.