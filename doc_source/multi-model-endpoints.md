# Host multiple models in one container behind one endpoint<a name="multi-model-endpoints"></a>

To create an endpoint that can host multiple models, use multi\-model endpoints\. Multi\-model endpoints provide a scalable and cost\-effective solution to deploying large numbers of models\. They use a shared serving container that is enabled to host multiple models\. This reduces hosting costs by improving endpoint utilization compared with using single\-model endpoints\. It also reduces deployment overhead because Amazon SageMaker manages loading models in memory and scaling them based on the traffic patterns to them\.

Multi\-model endpoints also enable time\-sharing of memory resources across your models\. This works best when the models are fairly similar in size and invocation latency\. When this is the case, multi\-model endpoints can effectively use instances across all models\. If you have models that have significantly higher transactions per second \(TPS\) or latency requirements, we recommend hosting them on dedicated endpoints\. Multi\-model endpoints are also well suited to scenarios that can tolerate occasional cold\-start\-related latency penalties that occur when invoking infrequently used models\.

Multi\-model endpoints support A/B testing\. They work with Auto Scaling and AWS PrivateLink\. You can use multi\-model\-enabled containers with serial inference pipelines, but only one multi\-model\-enabled container can be included in an inference pipeline\. You can't use multi\-model\-enabled containers with Amazon Elastic Inference\.

You can use the AWS SDK for Python \(Boto\) or the SageMaker console to create a multi\-model endpoint\. You can use multi\-model endpoints with custom\-built containers by integrating the [Multi Model Server](https://github.com/awslabs/multi-model-server) library\.

**Topics**
+ [Supported Algorithms and Frameworks](#multi-model-support)
+ [Sample Notebooks for Multi\-Model Endpoints](#multi-model-endpoint-sample-notebooks)
+ [How Multi\-Model Endpoints Work](#how-multi-mode-endpoints-work)
+ [Setting SageMaker Multi\-Model Endpoint Model Caching Behavior](#multi-model-caching)
+ [Instance Recommendations for Multi\-Model Endpoint Deployments](#multi-model-endpoint-instance)
+ [Create a Multi\-Model Endpoint](create-multi-model-endpoint.md)
+ [Invoke a Multi\-Model Endpoint](invoke-multi-model-endpoint.md)
+ [Add or Remove Models](add-models-to-endpoint.md)
+ [Build Your Own Container with Multi Model Server](build-multi-model-build-container.md)
+ [Multi\-Model Endpoint Security](multi-model-endpoint-security.md)
+ [CloudWatch Metrics for Multi\-Model Endpoint Deployments](multi-model-endpoint-cloudwatch-metrics.md)

## Supported Algorithms and Frameworks<a name="multi-model-support"></a>

The inference containers for the following algorithms and frameworks support multi\-model endpoints:
+ [XGBoost Algorithm](xgboost.md)
+ [K\-Nearest Neighbors \(k\-NN\) Algorithm](k-nearest-neighbors.md)
+ [Linear Learner Algorithm](linear-learner.md)
+ [Random Cut Forest \(RCF\) Algorithm](randomcutforest.md)
+ [Use TensorFlow with Amazon SageMaker](tf.md)
+ [Use Scikit\-learn with Amazon SageMaker](sklearn.md)
+ [Use Apache MXNet with Amazon SageMaker](mxnet.md)
+ [Use PyTorch with Amazon SageMaker](pytorch.md)

To use any other framework or algorithm, use the SageMaker inference toolkit to build a container that supports multi\-model endpoints\. For information, see [Build Your Own Container with Multi Model Server](build-multi-model-build-container.md)\.

## Sample Notebooks for Multi\-Model Endpoints<a name="multi-model-endpoint-sample-notebooks"></a>

For a sample notebook that uses SageMaker to deploy multiple XGBoost models to an endpoint, see the [Multi\-Model Endpoint XGBoost Sample Notebook](https://sagemaker-examples.readthedocs.io/en/latest/advanced_functionality/multi_model_xgboost_home_value/xgboost_multi_model_endpoint_home_value.html)\. For a sample notebook that shows how to set up and deploy a custom container that supports multi\-model endpoints in SageMaker, see the [Multi\-Model Endpoint BYOC Sample Notebook](https://sagemaker-examples.readthedocs.io/en/latest/advanced_functionality/multi_model_bring_your_own/multi_model_endpoint_bring_your_own.html)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you've created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all the SageMaker samples\. The Multi\-Model Endpoint notebook is located in the **ADVANCED FUNCTIONALITY** section\. To open a notebook, choose its **Use** tab and choose **Create copy**\.

## How Multi\-Model Endpoints Work<a name="how-multi-mode-endpoints-work"></a>

SageMaker manages the lifecycle of models hosted on multi\-model endpoints in the container's memory\. Instead of downloading all of the models from an Amazon S3 bucket to the container when you create the endpoint, SageMaker dynamically loads them when you invoke them\. When SageMaker receives an invocation request for a particular model, it does the following:

1. Routes the request to an instance behind the endpoint\.

1. Downloads the model from the S3 bucket to that instance's storage volume\.

1. Loads the model to the container's memory on that instance\. If the model is already loaded in the container's memory, invocation is faster because SageMaker doesn't need to download and load it\.

SageMaker continues to route requests for a model to the instance where the model is already loaded\. However, if the model receives many invocation requests, and there are additional instances for the multi\-model endpoint, SageMaker routes some requests to another instance to accommodate the traffic\. If the model isn't already loaded on the second instance, the model is downloaded to that instance's storage volume and loaded into the container's memory\.

When an instance's memory utilization is high and SageMaker needs to load another model into memory, it unloads unused models from that instance's container to ensure that there is enough memory to load the model\. Models that are unloaded remain on the instance's storage volume and can be loaded into the container's memory later without being downloaded again from the S3 bucket\. If the instance's storage volume reaches its capacity, SageMaker deletes any unused models from the storage volume\.

To delete a model, stop sending requests and delete it from the S3 bucket\. SageMaker provides multi\-model endpoint capability in a serving container\. Adding models to, and deleting them from, a multi\-model endpoint doesn't require updating the endpoint itself\. To add a model, you upload it to the S3 bucket and invoke it\. You don’t need code changes to use it\.

When you update a multi\-model endpoint, invocation requests on the endpoint might experience higher latencies as traffic is directed to the instances in the updated endpoint\.

## Setting SageMaker Multi\-Model Endpoint Model Caching Behavior<a name="multi-model-caching"></a>

By default, multi\-model endpoints cache frequently used models in memory and on disk to provide low latency inference\. The cached models are unloaded and/or deleted from disk only when a container runs out of memory or disk space to accommodate a newly targeted model\.

You can change the caching behavior of a multi\-model endpoint and explicitly enable or disable model caching by setting the parameter `ModelCacheSetting` when you call [create\_model](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model)\.

We recommend setting the value of the `ModelCacheSetting` parameter to `Disabled` for use cases that do not benefit from model caching\. For example, when a large number of models need to be served from the endpoint but each model is invoked only once \(or very infrequently\)\. For such use cases, setting the value of the `ModelCacheSetting` parameter to `Disabled` allows higher transactions per second \(TPS\) for `invoke_endpoint` requests comparted to the default caching mode\. Higher TPS in these use cases is because SageMaker does the following after the `invoke_endpoint` request:
+ Asynchronously unloads the model from memory and deletes it from disk immediately after it is invoked\.
+ Provides higher concurrency for downloading and loading models in the inference container\. The concurrency is a factor of the number of vCPUs of the container instance\.

For guidelines on choosing a SageMaker ML instance type for a multi\-model endpoint, see [Instance Recommendations for Multi\-Model Endpoint Deployments ](#multi-model-endpoint-instance)\.

## Instance Recommendations for Multi\-Model Endpoint Deployments<a name="multi-model-endpoint-instance"></a>

There are several items to consider when selecting a SageMaker ML instance type for a multi\-model endpoint\. Provision sufficient [Amazon Elastic Block Store \(Amazon EBS\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) capacity for all of the models that need to be served\. Balance performance \(minimize cold starts\) and cost \(don’t over\-provision instance capacity\)\. For information about the size of the storage volume that SageMaker attaches for each instance type for an endpoint and for a multi\-model endpoint, see [Host instance storage volumes](host-instance-storage.md)\. For a container configured to run in `MultiModel` mode, the storage volume provisioned for its instances has more memory\. This allows more models to be cached on the instance storage volume\. 

When choosing a SageMaker ML instance type, consider the following:
+ Multi\-model endpoints are not supported on GPU instance types\.
+ The traffic distribution \(access patterns\) to the models that you want to host behind the multi\-model endpoint, along with the model size \(how many models could be loaded in memory on the instance\):
  + Think of the amount of memory on an instance as the cache space for models to be loaded\. Think of the number of vCPUs as the concurrency limit to perform inference on the loaded models \(assuming that invoking a model is bound to CPU\)\.
  +  A higher amount of instance memory enables you to have more models loaded and ready to serve inference requests\. You don't need to waste time loading the model\.
  + A higher amount of vCPUs enables you to invoke more unique models concurrently \(again assuming that inference is bound to CPU\)\.
  + Have some "slack" memory available so that unused models can be unloaded, and especially for multi\-model endpoints with multiple instances\. If an instance or an Availability Zone fails, the models on those instances will be rerouted to other instances behind the endpoint\.
+ Tolerance to loading/downloading times:
  + d instance type families \(for example, m5d, c5d, or r5d\) come with an NVMe \(non\-volatile memory express\) SSD, which offers high I/O performance and might reduce the time it takes to download models to the storage volume and for the container to load the model from the storage volume\.
  + Because d instance types come with an NVMe SSD storage, SageMaker does not attach an Amazon EBS storage volume to these ML compute instances that hosts the multi\-model endpoint\. Auto scaling works best when the models are similarly sized and homogenous, that is when they have similar inference latency and resource requirements\.

In some cases, you might opt to reduce costs by choosing an instance type that can't hold all of the targeted models in memory at once\. SageMaker dynamically unloads models when it runs out of memory to make room for a newly targeted model\. For infrequently requested models, you are going to pay a price with the dynamic load latency\. In cases with more stringent latency needs, you might opt for larger instance types or more instances\. Investing time up front for proper performance testing and analysis will pay great dividends in successful production deployments\.

You can use the `Average` statistic of the `ModelCacheHit` metric to monitor the ratio of requests where the model is already loaded\. You can use the `SampleCount` statistic for the `ModelUnloadingTime` metric to monitor the number of unload requests sent to the container during a time period\. If models are unloaded too frequently \(an indicator of thrashing, where models are being unloaded and loaded again because there is insufficient cache space for the working set of models\), consider using a larger instance type with more memory or increasing the number of instances behind the multi\-model endpoint\. For multi\-model endpoints with multiple instances, be aware that a model might be loaded on more than 1 instance\. 

SageMaker multi\-model endpoints fully supports Auto Scaling, which manages replicas of models to ensure models scale based on traffic patterns\. We recommend that you configure your multi\-model endpoint and the size of your instances by considering all of the above and also set up auto scaling for your endpoint\. The invocation rates used to trigger an auto\-scale event is based on the aggregate set of predictions across the full set of models served by the endpoint\. 