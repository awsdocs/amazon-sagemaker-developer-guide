# Host Multiple Models with Multi\-Model Endpoints<a name="multi-model-endpoints"></a>

To create an endpoint that can host multiple models, use multi\-model endpoints\. Multi\-model endpoints provide a scalable and cost\-effective solution to deploying large numbers of models\. They use a shared serving container that is enabled to host multiple models\. This reduces hosting costs by improving endpoint utilization compared with using single\-model endpoints\. It also reduces deployment overhead because Amazon SageMaker manages loading models in memory and scaling them based on the traffic patterns to them\. 

Multi\-model endpoints also enable time\-sharing of memory resources across your models\. This works best when the models are fairly similar in size and invocation latency\. When this is the case, multi\-model endpoints can effectively use instances across all models\. If you have models that have significantly higher transactions per second \(TPS\) or latency requirements, we recommend hosting them on dedicated endpoints\. Multi\-model endpoints are also well suited to cases that can tolerate occasional cold\-start\-related latency penalties that occur when invoking infrequently used models\.

Multi\-model endpoints support A/B testing\. They work with Auto Scaling and AWS PrivateLink\. However, you can't use multi\-model\-enabled containers with serial inference pipelines or with Amazon Elastic Inference \(EIA\)\.

You can use the AWS SDK for Python \(Boto\) or the Amazon SageMaker console to create a multi\-model endpoint\. You can use multi\-model endpoints with custom\-built containers by integrating the [Multi Model Server](https://github.com/awslabs/multi-model-server) library\. 

**Topics**
+ [Sample Notebooks for Multi\-Model Endpoints](#multi-model-endpoint-sample-notebooks)
+ [How Multi\-Model Endpoints Work](#how-multi-mode-endpoints-work)
+ [Multi\-Model Endpoint Security](#multi-model-endpoint-security)
+ [Instance Recommendations for Multi\-Model Endpoint Deployments](#multi-model-endpoint-instance)
+ [CloudWatch Metrics for Multi\-Model Endpoint Deployments](#multi-model-endpoint-cloudwatch-metrics)
+ [Create a Multi\-Model Endpoint](create-multi-model-endpoint.md)
+ [Build Your Own Container with Multi Model Server](build-multi-model-build-container.md)
+ [Invoke a Multi\-Model Endpoint](invoke-multi-model-endpoint.md)
+ [Add or Remove Models](add-models-to-endpoint.md)

## Sample Notebooks for Multi\-Model Endpoints<a name="multi-model-endpoint-sample-notebooks"></a>

For a sample notebook that uses Amazon SageMaker to deploy multiple XGBoost models to an endpoint, see the [Multi\-Model Endpoint XGBoost Sample Notebook](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/multi_model_xgboost_home_value/xgboost_multi_model_endpoint_home_value.ipynb)\. For a sample notebook that shows how to set up and deploy a custom container that supports multi\-model endpoints in Amazon SageMaker, see the [Multi\-Model Endpoint BYOC Sample Notebook](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/multi_model_bring_your_own/multi_model_endpoint_bring_your_own.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in Amazon SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you've created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all the Amazon SageMaker samples\. The Multi\-Model Endpoint notebook is located in the **ADVANCED FUNCTIONALITY** section\. To open a notebook, choose its **Use** tab and choose **Create copy**\.

## How Multi\-Model Endpoints Work<a name="how-multi-mode-endpoints-work"></a>

Amazon SageMaker manages the lifecycle of models hosted on multi\-model endpoints in the container's memory\. Instead of downloading all of the models from an Amazon S3 bucket to the container when you create the endpoint, Amazon SageMaker dynamically loads them when you invoke them\. When Amazon SageMaker receives an invocation request for a particular model, it does the following:

1. Routes the request to a single Amazon EC2 instance behind the endpoint\.

1. Downloads the model from the S3 bucket to that instance's storage volume\.

1. Loads the model to the container's memory on that instance\. If the model is already loaded in the container's memory, invocation is faster because Amazon SageMaker doesn't need to download and load it\.

Amazon SageMaker continues to route requests for a model to the instance where the model is already loaded\. However, if the model receives many invocation requests, and there are additional instances for the multi\-model endpoint, Amazon SageMaker routes some requests to another instance to accommodate the traffic\. If the model isn't already loaded on the second instance, the model is downloaded to that instance's storage volume and loaded into the container's memory\.

When an instance's memory utilization is high and Amazon SageMaker needs to load another model into memory, it unloads unused models from that instance's container to ensure that there is enough memory to load the model\. Models that are unloaded remain on the instance's storage volume and can be loaded into the container's memory later without being downloaded again from the S3 bucket\. If the instance's storage volume reaches its capacity, Amazon SageMaker deletes any unused models from the storage volume\.

Adding models to, and deleting them from, a multi\-model endpoint doesn't require updating the endpoint itself\. To add a model, you upload it to the S3 bucket and invoke it\. To delete a model, stop sending requests and delete it from the S3 bucket\. Amazon SageMaker provides multi\-model endpoint capability in a serving container\. You don’t need code changes to use it\.

When you update a multi\-model endpoint, [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html) invocation requests on the endpoint might experience higher latencies as traffic is directed to the instances in the updated endpoint\.

## Multi\-Model Endpoint Security<a name="multi-model-endpoint-security"></a>

Models and data in a multi\-model endpoint are co\-located on instance storage volume and in container memory\. All instances for Amazon SageMaker endpoints run on a single tenant container that you own\. Only your models can run on your multi\-model endpoint\. It's your responsibility to manage the mapping of requests to models and to provide access for users to the correct target models\. Amazon SageMaker uses [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) to provide IAM identity\-based policies that you use to specify allowed or denied actions and resources and the conditions under which actions are allowed or denied\.

An IAM principal with [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html) permissions on a multi\-model endpoint can invoke any model at the address of the S3 prefix defined in the [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) operation, provided that the IAM Execution Role defined in operation has permissions to download the model\. If you need to restrict [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html) access to a limited set of models in S3, you can create multi\-model endpoints with more restrictive S3 prefixes\. For more information about how Amazon SageMaker uses roles to manage access to endpoints and perform operations on your behalf, see [Amazon SageMaker Roles ](sagemaker-roles.md)\. Your customers might also have certain data isolation requirements dictated by their own compliance requirements that can be satisfied using IAM identities\.

## Instance Recommendations for Multi\-Model Endpoint Deployments<a name="multi-model-endpoint-instance"></a>

There are several items to consider when selecting a SageMaker ML instance type for a multi\-model endpoint\. Provision sufficient [Amazon Elastic Block Store \(Amazon EBS\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) capacity for all of the models that need to be served\. Balance performance \(minimize cold starts\) and cost \(don’t over\-provision instance capacity\)\. For information about the size of the storage volume that Amazon SageMaker attaches for each instance type for an endpoint and for a multi\-model endpoint, see [Host Instance Storage Volumes](host-instance-storage.md)\. For a container configured to run in `MultiModel` mode, the storage volume provisioned for its instances has more memory\. This allows more models to be cached on the instance storage volume\. 

When choosing an Amazon SageMaker ML instance type, consider the following:
+ The traffic distribution \(access patterns\) to the models that you want to host behind the multi\-model endpoint, along with the model size \(how many models could be loaded in memory on the instance\)\.:
  + Think of the amount of memory on an instance as the cache space for models to be loaded\. Think of the number of vCPUs as the concurrency limit to perform inference on the loaded models \(assuming that invoking a model is bound to CPU\)\.
  +  A higher amount of instance memory allows you to have more models loaded and ready to serve inference requests\. You don't need to waste time loading the model\.
  + A higher amount of vCPUs allows you to invoke more unique models concurrently \(again assuming that inference is bound to CPU\)\.
  + Have some "slack" memory available so that unused models can be unloaded, and especially for multi\-model endpoints with multiple instances\. If an instance or an Availability Zone fails, the models on those instances will be rerouted to other instances behind the endpoint\.
+ Tolerance to loading/downloading times:
  + d instance type families \(for example, m5d, c5d, or r5d\) come with an NVMe \(non\-volatile memory express\) SSD, which offers high I/O performance and might reduce the time it takes to download models to the storage volume and for the container to load the model from the storage volume\.
  + Because d instance types come with an NVMe SSD storage, Amazon SageMaker does not attach an Amazon EBS storage volume to these ML compute instances that hosts the multi\-model endpoint\. \.

In some cases, you might opt to reduce costs by choosing an instance type that can't hold all of the targeted models in memory at once\. Amazon SageMaker dynamically unloads models when it runs out of memory to make room for a newly targeted model\. For infrequently requested models, you are going to pay a price with the dynamic load latency\. In cases with more stringent latency needs, you might opt for larger instance types or more instances\. Investing time up front for proper performance testing and analysis will pay great dividends in successful production deployments\.

You can use the `Average` statistic of the `ModelCacheHit` metric to monitor the ratio of requests where the model is already loaded\. You can use the `SampleCount` statistic for the `ModelUnloadingTime` metric to monitor the number of unload requests sent to the container during a time period\. If models are unloaded too frequently \(an indicator of thrashing, where models are being unloaded and loaded again because there is insufficient cache space for the working set of models\), consider using a larger instance type with more memory or increasing the number of instances behind the multi\-model endpoint\. For multi\-model endpoints with multiple instances, be aware that a model might be loaded on more than 1 instance\. 

Amazon SageMaker multi\-model endpoints fully supports Auto Scaling, which manages replicas of models to ensure models scale based on traffic patterns\. We recommend that you configure your multi\-model endpoint and the size of your instances by considering all of the above and also set up auto scaling for your endpoint\. The invocation rates used to trigger an auto\-scale event is based on the aggregate set of predictions across the full set of models served by the endpoint\. 

## CloudWatch Metrics for Multi\-Model Endpoint Deployments<a name="multi-model-endpoint-cloudwatch-metrics"></a>

Amazon SageMaker provides metrics for endpoints so you can monitor the cache hit rate, the number of models loaded, and the model wait times for loading, downloading, and uploading at a multi\-model endpoint\. For information, see **Multi\-Model Endpoint Model Loading Metrics** and **Multi\-Model Endpoint Model Instance Metrics** in [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md)\. Per\-model metrics aren't supported\. 