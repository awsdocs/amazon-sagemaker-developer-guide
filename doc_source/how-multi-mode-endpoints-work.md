# How Multi\-Model Endpoints Work<a name="how-multi-mode-endpoints-work"></a>

SageMaker manages the lifecycle of models hosted on multi\-model endpoints in the container's memory\. Instead of downloading all of the models from an Amazon S3 bucket to the container when you create the endpoint, SageMaker dynamically loads them when you invoke them\. When SageMaker receives an invocation request for a particular model, it does the following:

1. Routes the request to an instance behind the endpoint\.

1. Downloads the model from the S3 bucket to that instance's storage volume\.

1. Loads the model to the container's memory on that instance\. If the model is already loaded in the container's memory, invocation is faster because SageMaker doesn't need to download and load it\.

SageMaker continues to route requests for a model to the instance where the model is already loaded\. However, if the model receives many invocation requests, and there are additional instances for the multi\-model endpoint, SageMaker routes some requests to another instance to accommodate the traffic\. If the model isn't already loaded on the second instance, the model is downloaded to that instance's storage volume and loaded into the container's memory\.

When an instance's memory utilization is high and SageMaker needs to load another model into memory, it unloads unused models from that instance's container to ensure that there is enough memory to load the model\. Models that are unloaded remain on the instance's storage volume and can be loaded into the container's memory later without being downloaded again from the S3 bucket\. If the instance's storage volume reaches its capacity, SageMaker deletes any unused models from the storage volume\.

To delete a model, stop sending requests and delete it from the S3 bucket\. SageMaker provides multi\-model endpoint capability in a serving container\. Adding models to, and deleting them from, a multi\-model endpoint doesn't require updating the endpoint itself\. To add a model, you upload it to the S3 bucket and invoke it\. You don’t need code changes to use it\.

When you update a multi\-model endpoint, invocation requests on the endpoint might experience higher latencies as traffic is directed to the instances in the updated endpoint\.