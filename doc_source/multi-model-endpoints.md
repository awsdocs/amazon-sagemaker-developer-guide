# Host multiple models in one container behind one endpoint<a name="multi-model-endpoints"></a>

Multi\-model endpoints provide a scalable and cost\-effective solution to deploying large numbers of models\. They use the same fleet of resources and a shared serving container to host all of your models\. This reduces hosting costs by improving endpoint utilization compared with using single\-model endpoints\. It also reduces deployment overhead because Amazon SageMaker manages loading models in memory and scaling them based on the traffic patterns to your endpoint\.

The following diagram shows how multi\-model endpoints work compared to single\-model endpoints\.

![\[Diagram that shows how multi-model endpoints host models versus how single-model endpoints host models.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/multi-model-endpoints-diagram.png)

Multi\-model endpoints are ideal for hosting a large number of models that use the same ML framework on a shared serving container\. If you have a mix of frequently and infrequently accessed models, a multi\-model endpoint can efficiently serve this traffic with fewer resources and higher cost savings\. Your application should be tolerant of occasional cold start\-related latency penalties that occur when invoking infrequently used models\.

Multi\-model endpoints support hosting both CPU and GPU backed models\. By using GPU backed models, you can lower your model deployment costs through increased usage of the endpoint and its underlying accelerated compute instances\.

Multi\-model endpoints also enable time\-sharing of memory resources across your models\. This works best when the models are fairly similar in size and invocation latency\. When this is the case, multi\-model endpoints can effectively use instances across all models\. If you have models that have significantly higher transactions per second \(TPS\) or latency requirements, we recommend hosting them on dedicated endpoints\.

You can use multi\-model endpoints with the following features:
+ [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-services-overview.html) and VPCs
+ [Auto scaling](multi-model-endpoints-autoscaling.md)
+ [Serial inference pipelines](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipelines.html) \(but only one multi\-model enabled container can be included in an inference pipeline\)
+ A/B testing

You can't use multi\-model\-enabled containers with Amazon Elastic Inference\.

You can use the AWS SDK for Python \(Boto\) or the SageMaker console to create a multi\-model endpoint\. For CPU backed multi\-model endpoints, you can create your endpoint with custom\-built containers by integrating the [Multi Model Server](https://github.com/awslabs/multi-model-server) library\.

**Topics**
+ [Supported algorithms, frameworks, and instances](#multi-model-support)
+ [Sample notebooks for multi\-model endpoints](#multi-model-endpoint-sample-notebooks)
+ [How multi\-model endpoints work](#how-multi-model-endpoints-work)
+ [Setting SageMaker multi\-model endpoint model caching behavior](#multi-model-caching)
+ [Instance recommendations for multi\-model endpoint deployments](#multi-model-endpoint-instance)
+ [Create a Multi\-Model Endpoint](create-multi-model-endpoint.md)
+ [Invoke a Multi\-Model Endpoint](invoke-multi-model-endpoint.md)
+ [Add or Remove Models](add-models-to-endpoint.md)
+ [Build Your Own Container for SageMaker Multi\-Model Endpoints](build-multi-model-build-container.md)
+ [Multi\-Model Endpoint Security](multi-model-endpoint-security.md)
+ [CloudWatch Metrics for Multi\-Model Endpoint Deployments](multi-model-endpoint-cloudwatch-metrics.md)
+ [Set Auto Scaling Policies for Multi\-Model Endpoint Deployments](multi-model-endpoints-autoscaling.md)

## Supported algorithms, frameworks, and instances<a name="multi-model-support"></a>

For information about the algorithms, frameworks, and instance types that you can use with multi\-model endpoints, see the following sections\.

### Supported algorithms, frameworks, and instances for multi\-model endpoints using CPU backed instances<a name="multi-model-support-cpu"></a>

The inference containers for the following algorithms and frameworks support multi\-model endpoints:
+ [XGBoost Algorithm](xgboost.md)
+ [K\-Nearest Neighbors \(k\-NN\) Algorithm](k-nearest-neighbors.md)
+ [Linear Learner Algorithm](linear-learner.md)
+ [Random Cut Forest \(RCF\) Algorithm](randomcutforest.md)
+ [Use TensorFlow with Amazon SageMaker](tf.md)
+ [Use Scikit\-learn with Amazon SageMaker](sklearn.md)
+ [Use Apache MXNet with Amazon SageMaker](mxnet.md)
+ [Use PyTorch with Amazon SageMaker](pytorch.md)

To use any other framework or algorithm, use the SageMaker inference toolkit to build a container that supports multi\-model endpoints\. For information, see [Build Your Own Container for SageMaker Multi\-Model Endpoints](build-multi-model-build-container.md)\.

Multi\-model endpoints support all of the CPU instance types\.

### Supported algorithms, frameworks, and instances for multi\-model endpoints using GPU backed instances<a name="multi-model-support-gpu"></a>

Hosting multiple GPU backed models on multi\-model endpoints is supported through the [SageMaker Triton Inference server](https://docs.aws.amazon.com/sagemaker/latest/dg/triton.html)\. This supports all major inference frameworks such as NVIDIA® TensorRT™, PyTorch, MXNet, Python, ONNX, XGBoost, scikit\-learn, RandomForest, OpenVINO, custom C\+\+, and more\.

To use any other framework or algorithm, you can use Triton backend for Python or C\+\+ to write your model logic and serve any custom model\. After you have the server ready, you can start deploying 100s of Deep Learning models behind one endpoint\.

Multi\-model endpoints support the following GPU instance types:


| Instance family | Instance type | vCPUs | GiB of memory per vCPU | GPUs | GPU memory | 
| --- | --- | --- | --- | --- | --- | 
| p2 | ml\.p2\.xlarge | 4 | 15\.25 | 1 | 12 | 
| p3 | ml\.p3\.2xlarge | 8 | 7\.62 | 1 | 16 | 
| g5 | ml\.g5\.xlarge | 4 | 4 | 1 | 24 | 
| g5 | ml\.g5\.2xlarge | 8 | 4 | 1 | 24 | 
| g5 | ml\.g5\.4xlarge | 16 | 4 | 1 | 24 | 
| g5 | ml\.g5\.8xlarge | 32 | 4 | 1 | 24 | 
| g5 | ml\.g5\.16xlarge | 64 | 4 | 1 | 24 | 
| g4dn | ml\.g4dn\.xlarge | 4 | 4 | 1 | 16 | 
| g4dn | ml\.g4dn\.2xlarge | 8 | 4 | 1 | 16 | 
| g4dn | ml\.g4dn\.4xlarge | 16 | 4 | 1 | 16 | 
| g4dn | ml\.g4dn\.8xlarge | 32 | 4 | 1 | 16 | 
| g4dn | ml\.g4dn\.16xlarge | 64 | 4 | 1 | 16 | 

## Sample notebooks for multi\-model endpoints<a name="multi-model-endpoint-sample-notebooks"></a>

To learn more about how to use multi\-model endpoints, you can try the following sample notebooks:
+ Examples for multi\-model endpoints using CPU backed instances:
  + [Multi\-Model Endpoint XGBoost Sample Nootebook](https://sagemaker-examples.readthedocs.io/en/latest/advanced_functionality/multi_model_xgboost_home_value/xgboost_multi_model_endpoint_home_value.html) – This notebook shows how to deploy multiple XGBoost models to an endpoint\.
  + [Multi\-Model Endpoints BYOC Sample Notebook](https://sagemaker-examples.readthedocs.io/en/latest/advanced_functionality/multi_model_bring_your_own/multi_model_endpoint_bring_your_own.html) – This notebook shows how to set up and deploy a customer container that supports multi\-model endpoints in SageMaker\.
+ Example for multi\-model endpoints using GPU backed instances:
  + [Run mulitple deep learning models on GPUs with Amazon SageMaker Multi\-model endpoints \(MME\)](https://github.com/aws/amazon-sagemaker-examples/blob/main/multi-model-endpoints/mme-on-gpu/cv/resnet50_mme_with_gpu.ipynb) – This notebook shows how to use an NVIDIA Triton Inference container to deploy ResNet\-50 models to a multi\-model endpoint\.

For instructions on how to create and access Jupyter notebook instances that you can use to run the previous examples in SageMaker, see [Amazon SageMaker Notebook Instances](nbi.md)\. After you've created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all the SageMaker samples\. The multi\-model endpoint notebooks are located in the **ADVANCED FUNCTIONALITY** section\. To open a notebook, choose its **Use** tab and choose **Create copy**\.

For more information about use cases for multi\-model endpoints, see the following blogs and resources:
+ Video: [Hosting thousands of models on SageMaker](https://www.youtube.com/watch?v=XqCNTWmHsLc&t=751s)
+ Video: [SageMaker ML for SaaS](https://www.youtube.com/watch?v=BytpYlJ3vsQ)
+ Blog: [How to scale machine learning inference for multi\-tenant SaaS use cases](http://aws.amazon.com/blogs/machine-learning/how-to-scale-machine-learning-inference-for-multi-tenant-saas-use-cases/)
+ Case study: [Veeva Systems](http://aws.amazon.com/partners/success/advanced-clinical-veeva/)

## How multi\-model endpoints work<a name="how-multi-model-endpoints-work"></a>

 SageMaker manages the lifecycle of models hosted on multi\-model endpoints in the container's memory\. Instead of downloading all of the models from an Amazon S3 bucket to the container when you create the endpoint, SageMaker dynamically loads and caches them when you invoke them\. When SageMaker receives an invocation request for a particular model, it does the following: 

1. Routes the request to an instance behind the endpoint\.

1. Downloads the model from the S3 bucket to that instance's storage volume\.

1. Loads the model to the container's memory \(CPU or GPU, depending on whether you have CPU or GPU backed instances\) on that accelerated compute instance\. If the model is already loaded in the container's memory, invocation is faster because SageMaker doesn't need to download and load it\.

SageMaker continues to route requests for a model to the instance where the model is already loaded\. However, if the model receives many invocation requests, and there are additional instances for the multi\-model endpoint, SageMaker routes some requests to another instance to accommodate the traffic\. If the model isn't already loaded on the second instance, the model is downloaded to that instance's storage volume and loaded into the container's memory\.

When an instance's memory utilization is high and SageMaker needs to load another model into memory, it unloads unused models from that instance's container to ensure that there is enough memory to load the model\. Models that are unloaded remain on the instance's storage volume and can be loaded into the container's memory later without being downloaded again from the S3 bucket\. If the instance's storage volume reaches its capacity, SageMaker deletes any unused models from the storage volume\.

To delete a model, stop sending requests and delete it from the S3 bucket\. SageMaker provides multi\-model endpoint capability in a serving container\. Adding models to, and deleting them from, a multi\-model endpoint doesn't require updating the endpoint itself\. To add a model, you upload it to the S3 bucket and invoke it\. You don’t need code changes to use it\.

**Note**  
When you update a multi\-model endpoint, initial invocation requests on the endpoint might experience higher latencies as Smart Routing in multi\-model endpoints adapt to your traffic pattern\. However, once it learns your traffic pattern, you can experience low latencies for most frequently used models\. Less frequently used models may incur some cold start latencies since the models are dynamically loaded to an instance\.

## Setting SageMaker multi\-model endpoint model caching behavior<a name="multi-model-caching"></a>

By default, multi\-model endpoints cache frequently used models in memory \(CPU or GPU, depending on whether you have CPU or GPU backed instances\) and on disk to provide low latency inference\. The cached models are unloaded and/or deleted from disk only when a container runs out of memory or disk space to accommodate a newly targeted model\.

You can change the caching behavior of a multi\-model endpoint and explicitly enable or disable model caching by setting the parameter `ModelCacheSetting` when you call [create\_model](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model)\.

We recommend setting the value of the `ModelCacheSetting` parameter to `Disabled` for use cases that do not benefit from model caching\. For example, when a large number of models need to be served from the endpoint but each model is invoked only once \(or very infrequently\)\. For such use cases, setting the value of the `ModelCacheSetting` parameter to `Disabled` allows higher transactions per second \(TPS\) for `invoke_endpoint` requests compared to the default caching mode\. Higher TPS in these use cases is because SageMaker does the following after the `invoke_endpoint` request:
+ Asynchronously unloads the model from memory and deletes it from disk immediately after it is invoked\.
+ Provides higher concurrency for downloading and loading models in the inference container\. For both CPU and GPU backed endpoints, the concurrency is a factor of the number of the vCPUs of the container instance\.

For guidelines on choosing a SageMaker ML instance type for a multi\-model endpoint, see [Instance recommendations for multi\-model endpoint deployments](#multi-model-endpoint-instance)\.

## Instance recommendations for multi\-model endpoint deployments<a name="multi-model-endpoint-instance"></a>

There are several items to consider when selecting a SageMaker ML instance type for a multi\-model endpoint:
+ Provision sufficient [ Amazon Elastic Block Store \(Amazon EBS\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) capacity for all of the models that need to be served\.
+ Balance performance \(minimize cold starts\) and cost \(don’t over\-provision instance capacity\)\. For information about the size of the storage volume that SageMaker attaches for each instance type for an endpoint and for a multi\-model endpoint, see [Host instance storage volumes](host-instance-storage.md)\.
+ For a container configured to run in `MultiModel` mode, the storage volume provisioned for its instances are larger than the default `SingleModel` mode\. This allows more models to be cached on the instance storage volume than in `SingleModel` mode\.

When choosing a SageMaker ML instance type, consider the following:
+ Multi\-model endpoints are currently supported for all CPU instances types and on single\-GPU instance types\.
+ For the traffic distribution \(access patterns\) to the models that you want to host behind the multi\-model endpoint, along with the model size \(how many models could be loaded in memory on the instance\), keep the following information in mind:
  + Think of the amount of memory on an instance as the cache space for models to be loaded, and think of the number of vCPUs as the concurrency limit to perform inference on the loaded models \(assuming that invoking a model is bound to CPU\)\.
  + For CPU backed instances, the number of vCPUs impacts your maximum concurrenct invocations per instance \(assuming that invoking a model is bound to CPU\)\. A higher amount of vCPUs enables you to invoke more unique models concurrently\.
  + For GPU backed instances, a higher amount of instance and GPU memory enables you to have more models loaded and ready to serve inference requests\.
  + For both CPU and GPU backed instances, have some "slack" memory available so that unused models can be unloaded, and especially for multi\-model endpoints with multiple instances\. If an instance or an Availability Zone fails, the models on those instances will be rerouted to other instances behind the endpoint\.
+ Determine your tolerance to loading/downloading times:
  + d instance type families \(for example, m5d, c5d, or r5d\) and g5s come with an NVMe \(non\-volatile memory express\) SSD, which offers high I/O performance and might reduce the time it takes to download models to the storage volume and for the container to load the model from the storage volume\.
  + Because d and g5 instance types come with an NVMe SSD storage, SageMaker does not attach an Amazon EBS storage volume to these ML compute instances that hosts the multi\-model endpoint\. Auto scaling works best when the models are similarly sized and homogenous, that is when they have similar inference latency and resource requirements\.

You can also use the following guidance to help you optimize model loading on your multi\-model endpoints:

**Choosing an instance type that can't hold all of the targeted models in memory**

In some cases, you might opt to reduce costs by choosing an instance type that can't hold all of the targeted models in memory at once\. SageMaker dynamically unloads models when it runs out of memory to make room for a newly targeted model\. For infrequently requested models, you sacrifice dynamic load latency\. In cases with more stringent latency needs, you might opt for larger instance types or more instances\. Investing time up front for performance testing and analysis helps you to have successful production deployments\.

**Evaluating your model cache hits**

Amazon CloudWatch metrics can help you evaluate your models\. For more information about metrics you can use with multi\-model endpoints, see [CloudWatch Metrics for Multi\-Model Endpoint Deployments ](multi-model-endpoint-cloudwatch-metrics.md)\.

 You can use the `Average` statistic of the `ModelCacheHit` metric to monitor the ratio of requests where the model is already loaded\. You can use the `SampleCount` statistic for the `ModelUnloadingTime` metric to monitor the number of unload requests sent to the container during a time period\. If models are unloaded too frequently \(an indicator of *thrashing*, where models are being unloaded and loaded again because there is insufficient cache space for the working set of models\), consider using a larger instance type with more memory or increasing the number of instances behind the multi\-model endpoint\. For multi\-model endpoints with multiple instances, be aware that a model might be loaded on more than 1 instance\.