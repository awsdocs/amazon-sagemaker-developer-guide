# Serverless Inference<a name="serverless-endpoints"></a>

Amazon SageMaker Serverless Inference is a purpose\-built inference option that makes it easy for you to deploy and scale ML models\. Serverless Inference is ideal for workloads which have idle periods between traffic spurts and can tolerate cold starts\. Serverless endpoints automatically launch compute resources and scale them in and out depending on traffic, eliminating the need to choose instance types or manage scaling policies\. This takes away the undifferentiated heavy lifting of selecting and managing servers\. Serverless Inference integrates with AWS Lambda to offer you high availability, built\-in fault tolerance and automatic scaling\.

With a pay\-per\-use model, Serverless Inference is a cost\-effective option if you have an infrequent or unpredictable traffic pattern\. During times when there are no requests, Serverless Inference scales your endpoint down to 0, helping you to minimize your costs\. For more information about pricing for Serverless Inference, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

You can integrate Serverless Inference with your MLOps Pipelines to streamline your ML workflow, and you can use a serverless endpoint to host a model registered with [Model Registry](model-registry.md)\.

Serverless Inference is generally available in all AWS commercial Regions where SageMaker is available \(except the AWS China Regions\)\. For more information about Amazon SageMaker regional availability, see the [AWS Regional Services List](http://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

## How it works<a name="serverless-endpoints-how-it-works"></a>

The following diagram shows the workflow of Serverless Inference and the benefits of using a serverless endpoint\.

![\[Diagram of the Serverless Inference workflow: the client sends a request to Serverless Inference and model predictions are sent back in response.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/serverless-endpoints-how-it-works.png)

When you create a serverless endpoint, SageMaker provisions and manages the compute resources for you\. Then, you can make inference requests to the endpoint and receive model predictions in response\. SageMaker scales the compute resources up and down as needed to handle your request traffic, and you only pay for what you use\.

The following sections provide additional details about Serverless Inference and how it works\.

**Topics**
+ [Container support](#serverless-endpoints-how-it-works-containers)
+ [Memory size](#serverless-endpoints-how-it-works-memory)
+ [Concurrent invocations](#serverless-endpoints-how-it-works-concurrency)
+ [Cold starts](#serverless-endpoints-how-it-works-cold-starts)
+ [Feature exclusions](#serverless-endpoints-how-it-works-exclusions)

### Container support<a name="serverless-endpoints-how-it-works-containers"></a>

For your endpoint container, you can choose either a SageMaker\-provided container or bring your own\. SageMaker provides containers for its built\-in algorithms and prebuilt Docker images for some of the most common machine learning frameworks, such as Apache MXNet, TensorFlow, PyTorch, and Chainer\. For a list of available SageMaker images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. If you are bringing your own container, you must modify it to work with SageMaker\. For more information about bringing your own container, see [Adapting Your Own Inference Container](adapt-inference-container.md)\.

For serverless endpoints, we recommend creating only one worker in the container and only loading one copy of the model\. Note that this is unlike real\-time endpoints, where some SageMaker containers may create a worker for each vCPU to process inference requests and load the model in each worker\.

If you already have a container for a real\-time endpoint, you can use the same container for your serverless endpoint, though some capabilities are excluded\. To learn more about the container capabilities that are not supported in Serverless Inference, see [Feature exclusions](#serverless-endpoints-how-it-works-exclusions)\. If you choose to use the same container, SageMaker escrows \(retains\) a copy of your container image until you delete all endpoints that use the image\. SageMaker encrypts the copied image at rest with a SageMaker\-owned AWS KMS key\.

### Memory size<a name="serverless-endpoints-how-it-works-memory"></a>

Your serverless endpoint has a minimum RAM size of 1024 MB \(1 GB\), and the maximum RAM size you can choose is 6144 MB \(6 GB\)\. The memory sizes you can choose are 1024 MB, 2048 MB, 3072 MB, 4096 MB, 5120 MB, or 6144 MB\. Serverless Inference auto\-assigns compute resources proportional to the memory you select\. If you choose a larger memory size, your container has access to more vCPUs\. Choose your endpoint’s memory size according to your model size\. Generally, the memory size should be at least as large as your model size\. You may need to benchmark in order to choose the right memory selection for your model based on your latency SLAs\. The memory size increments have different pricing; see the [Amazon SageMaker pricing page](https://aws.amazon.com/sagemaker/pricing/) for more information\.

Regardless of the memory size you choose, your serverless endpoint has 5 GB of ephemeral disk storage available\. For help with container permissions issues when working with storage, see [Troubleshooting](serverless-endpoints-troubleshooting.md)\.

### Concurrent invocations<a name="serverless-endpoints-how-it-works-concurrency"></a>

Serverless Inference manages predefined scaling policies and quotas for the capacity of your endpoint\. Serverless endpoints have a quota for how many concurrent invocations can be processed at the same time\. If the endpoint is invoked before it finishes processing the first request, then it handles the second request concurrently\. You can set the maximum concurrency for a single endpoint up to 200, and the total number of serverless endpoints you can host in a Region is 50\. The total concurrency you can share between all serverless endpoints per Region in your account is 200\. The maximum concurrency for an individual endpoint prevents that endpoint from taking up all of the invocations allowed for your account, and any endpoint invocations beyond the maximum are throttled\.

To learn how to set the maximum concurrency for your endpoint, see [Create an endpoint configuration](serverless-endpoints-create.md#serverless-endpoints-create-config)\. For more information about quotas and limits, see [ Amazon SageMaker endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html) in the *AWS General Reference*\. To request a service limit increase, contact [AWS Support](https://console.aws.amazon.com/support)\. For instructions on how to request a service limit increase, see [Supported Regions and Quotas](regions-quotas.md)\.

### Cold starts<a name="serverless-endpoints-how-it-works-cold-starts"></a>

If your endpoint does not receive traffic for a while and then your endpoint suddenly receives new requests, it can take some time for your endpoint to spin up the compute resources to process the requests\. This is called a *cold start*\. Since serverless endpoints provision compute resources on demand, your endpoint may experience cold starts\. A cold start can also occur if your concurrent requests exceed the current concurrent request usage\. The cold start time depends on your model size, how long it takes to download your model, and the start\-up time of your container\.

To monitor how long your cold start time is, you can use the Amazon CloudWatch metric `ModelSetupTime` to monitor your serverless endpoint\. This metric tracks the time it takes to launch new compute resources for your endpoint\. To learn more about using CloudWatch metrics with serverless endpoints, see [Monitor a Serverless Endpoint](serverless-endpoints-monitoring.md)\.

### Feature exclusions<a name="serverless-endpoints-how-it-works-exclusions"></a>

Some of the features currently available for SageMaker Real\-time Inference are not supported for Serverless Inference, including GPUs, AWS marketplace model packages, private Docker registries, Multi\-Model Endpoints, KMS keys, VPC configuration, network isolation, data capture, multiple production variants, Model Monitor, and inference pipelines\.

You cannot convert your instance\-based, real\-time endpoint to a serverless endpoint\. If you try to update your real\-time endpoint to serverless, you receive a `ValidationError` message\. You can convert a serverless endpoint to real\-time, but once you make the update, you cannot roll it back to serverless\.

## Getting started<a name="serverless-endpoints-get-started"></a>

You can create, update, describe, and delete a serverless endpoint using the SageMaker console, the AWS SDKs, the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/overview.html#sagemaker-serverless-inference), and the AWS CLI\. You can invoke your endpoint using the AWS SDKs, the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/overview.html#sagemaker-serverless-inference), and the AWS CLI\. For more information about how to set up and use a serverless endpoint, read the guide [Create, Invoke, Update, and Delete an Endpoint](serverless-endpoints-create-invoke-update-delete.md)\.

### Example notebooks and blogs<a name="serverless-endpoints-get-started-nbs"></a>

For Jupyter notebook examples that show end\-to\-end serverless endpoint workflows, see the [Serverless Inference example notebooks](https://github.com/aws/amazon-sagemaker-examples/tree/master/serverless-inference)\.