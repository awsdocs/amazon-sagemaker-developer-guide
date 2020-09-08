# Contract for Custom Containers to Serve Multiple Model<a name="mms-container-apis"></a>

To handle multiple models, your container must support a set of APIs that enable Amazon SageMaker to communicate with the container for loading, listing, getting, and unloading models as required\. The `model_name` is used in the new set of APIs as the key input parameter\. The customer container is expected to keep track of the loaded models using `model_name` as the mapping key\. Also, the `model_name` is an opaque identifier and is not necessarily the value of the `TargetModel` parameter passed into the `InvokeEndpoint` API\. The original `TargetModel` value in the `InvokeEndpoint` request is passed to container in the APIs as a `X-Amzn-SageMaker-Target-Model` header that can be used for logging purposes\.

**Topics**
+ [LOAD MODEL API](#multi-model-api-load-model)
+ [LIST MODEL API](#multi-model-api-list-model)
+ [GET MODEL API](#multi-model-api-get-model)
+ [UNLOAD MODEL API](#multi-model-api-unload-model)
+ [INVOKE MODEL API](#multi-model-api-invoke-model)

## LOAD MODEL API<a name="multi-model-api-load-model"></a>

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
If `model_name` is already loaded, this API should return 409\. Any time a model cannot be loaded due to lack of memory or to any other resource, this API should return a 507 HTTP status code to SageMaker, which then initiates unloading unused models to reclaim\.

## LIST MODEL API<a name="multi-model-api-list-model"></a>

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

SageMaker can initially call the List Models API without providing a value for `next_page_token`\. If a `nextPageToken` field is returned as part of the response, it will be provided as the value for `next_page_token` in a subsequent List Models call\. If a `nextPageToken` is not returned, it means that there are no more models to return\.

## GET MODEL API<a name="multi-model-api-get-model"></a>

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

## UNLOAD MODEL API<a name="multi-model-api-unload-model"></a>

Instructs the SageMaker platform to instruct the customer container to unload a model from memory\. This initiates the eviction of a candidate model as determined by the platform when starting the process of loading a new model\. The resources provisioned to `model_name` should be reclaimed by the container when this API returns a response\.

```
DELETE /models/{model_name}
```

**Note**  
If `model_name` is not loaded, this API should return 404\.

## INVOKE MODEL API<a name="multi-model-api-invoke-model"></a>

Makes a prediction request from the particular `model_name` supplied\. The SageMaker Runtime `InvokeEndpoint` request supports `X-Amzn-SageMaker-Target-Model` as a new header that takes the relative path of the model specified for invocation\. The SageMaker system constructs the absolute path of the model by combining the prefix that is provided as part of the `CreateModel` API call with the relative path of the model\.

```
POST /models/{model_name}/invoke HTTP/1.1
Content-Type: ContentType
Accept: Accept
X-Amzn-SageMaker-Custom-Attributes: CustomAttributes
X-Amzn-SageMaker-Target-Model: [relativePath]/{artifactName}.tar.gz
```

**Note**  
If `model_name` is not loaded, this API should return 404\.