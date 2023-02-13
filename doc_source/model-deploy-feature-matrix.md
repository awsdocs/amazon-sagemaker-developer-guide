# Supported features<a name="model-deploy-feature-matrix"></a>

 Amazon SageMaker offers the following four options to deploy models for inference\. 
+  Real\-time inference for inference workloads with real\-time, interactive, low latency requirements\. 
+  Batch transform for offline inference with large datasets\. 
+  Asynchronous inference for near\-real\-time inference with large inputs that require longer preprocessing times\. 
+  Serverless inference for inference workloads that have idle periods between traffic spurts\. 

 The following table summarizes the core platform features that are supported by each inference option\. It does not show features that can be provided by frameworks, custom Docker containers, or through chaining different AWS services\. 


| Feature | [Real\-time inference](realtime-endpoints.md) | [Batch transform](batch-transform.md) | [Asynchronous inference](async-inference.md) | [Serverless inference](serverless-endpoints.md) | [Docker containers](docker-containers.md) | 
| --- | --- | --- | --- | --- | --- | 
| [Autoscaling support](endpoint-auto-scaling.md) | ✓ | N/A | ✓ | ✓ | N/A | 
| GPU support | ✓1 | ✓1 | ✓1 |  | [1P](common-info-all-im-models.md), pre\-built, BYOC | 
| Single model | ✓ | ✓ | ✓ | ✓ | N/A | 
| [Multi\-model endpoint](multi-model-endpoints.md) | ✓ |  |  |  | k\-NN, XGBoost, Linear Learner, RCF, TensorFlow, Apache MXNet, PyTorch, scikit\-learn 2 | 
| [Multi\-container endpoint](multi-container-endpoints.md) | ✓ |  |  |  | 1P, pre\-built, Extend pre\-built, BYOC | 
| [Serial inference pipeline](inference-pipelines.md) | ✓ | ✓ |  |  | 1P, pre\-built, Extend pre\-built, BYOC | 
| [Inference Recommender](inference-recommender.md) | ✓ |  |  |  | 1P, pre\-built, Extend pre\-built, BYOC | 
| Private link support | ✓ | ✓ | ✓ |  | N/A | 
| [Data capture/Model monitor support](model-monitor.md) | ✓ | ✓ |  |  | N/A | 
| [DLCs supported](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) | 1P, pre\-built, Extend pre\-built, BYOC | [1P](common-info-all-im-models.md), pre\-built, Extend pre\-built, BYOC | 1P, pre\-built, Extend pre\-built, BYOC | 1P, pre\-built, Extend pre\-built, BYOC | N/A | 
| Protocols supported | HTTP\(S\) | HTTP\(S\) | HTTP\(S\) | HTTP\(S\) | N/A | 
| Payload size | < 6 MB | ≤ 100 MB | ≤ 1 GB | ≤ 4 MB |  | 
| HTTP chunked encoding | Framework dependent, 1P not supported | N/A | Framework dependent, 1P not supported | Framework dependent, 1P not supported | N/A | 
| Request timeout | < 60 seconds | Days | < 15 minutes | < 60 seconds | N/A | 
| [Deployment guardrails](deployment-guardrails.md) | ✓ |  | ✓ | All\-at\-once traffic fall back | N/A | 
| [Shadow testing](shadow-tests.md) | ✓ |  |  |  | N/A | 
| Scale to zero |  | N/A | ✓ | ✓ | N/A | 
| Market place model packages support | ✓ | ✓ |  |  | N/A | 
| Virtual private cloud support | ✓ | ✓ | ✓ |  | N/A | 
| Multiple production variants support | ✓ |  |  |  | N/A | 
| Network isolation | ✓ |  | ✓ |  | N/A | 
| [Model parallel serving support](model-parallel-intro.md) | ✓3 | ✓ | ✓3 |  | ✓3 | 
| Volume encryption | ✓ | ✓ | ✓ | ✓ | N/A | 
| Customer AWS KMS | ✓ | ✓ | ✓ | ✓ | N/A | 
| d instance support | ✓ | ✓ | ✓ |  | N/A | 
| [inf1 support](neo-supported-cloud.md) | ✓ |  |  |  | ✓ | 

 With SageMaker, you can deploy a single model, or multiple models behind a single inference endpoint for real\-time inference\. The following table summarizes the core features supported by various hosting options that come with real\-time inference\. 


| Feature | [Single model endpoints](realtime-single-model.md) | [Multi\-model endpoints](multi-model-endpoints.md) | [Serial inference pipeline](inference-pipelines.md) | [Multi\-container endpoints](multi-container-endpoints.md) | 
| --- | --- | --- | --- | --- | 
| [Autoscaling support](endpoint-auto-scaling.md) | ✓ | ✓ | ✓ | ✓ | 
| GPU support | ✓1 | ✓ | ✓ |  | 
| Single model | ✓ | ✓ | ✓ | ✓ | 
| [Multi\-model endpoints](multi-model-endpoints.md) |  | ✓ | ✓ | N/A | 
| [Multi\-container endpoints](multi-container-endpoints.md) | ✓ |  |  | N/A | 
| [Serial inference pipeline](inference-pipelines.md) | ✓ | ✓ | N/A |  | 
| [Inference Recommender](inference-recommender.md) | ✓ |  |  |  | 
| Private link support | ✓ | ✓ | ✓ | ✓ | 
| [Data capture/Model monitor support](model-monitor.md) | ✓ | N/A | N/A | N/A | 
| DLCs supported | 1P, pre\-built, Extend pre\-built, BYOC | k\-NN, XGBoost, Linear Learner, RCF, TensorFlow, Apache MXNet, PyTorch, scikit\-learn 2 | 1P, pre\-built, Extend pre\-built, BYOC | 1P, pre\-built, Extend pre\-built, BYOC | 
| Protocols supported | HTTP\(S\) | HTTP\(S\) | HTTP\(S\) | HTTP\(S\) | 
| Payload size | < 6 MB | < 6 MB | < 6 MB | < 6 MB | 
| Request timeout | < 60 seconds | < 60 seconds | < 60 seconds | < 60 seconds | 
| [Deployment guardrails](deployment-guardrails.md) | ✓ |  | ✓ | ✓ | 
| [Shadow testing](shadow-tests.md) | ✓ |  |  |  | 
| Market place model packages support | ✓ |  |  |  | 
| Virtual private cloud support | ✓ | ✓ | ✓ | ✓ | 
| Multiple production variants support | ✓ |  | ✓ | ✓ | 
| Network isolation | ✓ | ✓ | ✓ | ✓ | 
| [Model parallel serving support](model-parallel-intro.md) | ✓ 3 |  | ✓ 3 |  | 
| Volume encryption | ✓ | ✓ | ✓ | ✓ | 
| Customer AWS KMS | ✓ | ✓ | ✓ | ✓ | 
| d instance support | ✓ | ✓ | ✓ | ✓ | 
| [inf1 support](neo-supported-cloud.md) | ✓ |  |  |  | 

 1 Availability of the Amazon EC2 instance types depends on the AWS Region\. For availability of instances specific to AWS, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\. 

 2 To use any other framework or algorithm, use the SageMaker Inference toolkit to build a container that supports multi\-model endpoints\. 

 3 With SageMaker, you can deploy large models \(up to 500 GB\) for inference\. You can configure the container health check and download timeout quotas, up to 60 minutes\. This will allow you to have more time to download and load your model and associated resources\. For more information, see [SageMaker endpoint parameters for large model inference](realtime-endpoints-large-model-hosting.md)\. You can use SageMaker compatible [large model Inference containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#large-model-inference-containers)\. You can also use third\-party model parallelization libraries, such as Triton with FasterTransformer and DeepSpeed\. You have to ensure that they are compatible with SageMaker\. 