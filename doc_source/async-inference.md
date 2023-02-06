# Asynchronous inference<a name="async-inference"></a>

Amazon SageMaker Asynchronous Inference is a new capability in SageMaker that queues incoming requests and processes them asynchronously\. This option is ideal for requests with large payload sizes \(up to 1GB\), long processing times \(up to one hour\), and near real\-time latency requirements\. Asynchronous Inference enables you to save on costs by autoscaling the instance count to zero when there are no requests to process, so you only pay when your endpoint is processing requests\.

## How It Works<a name="async-inference-how-it-works"></a>

Creating an asynchronous inference endpoint is similar to creating real\-time inference endpoints\. You can use your existing SageMaker models and only need to specify the `AsyncInferenceConfig` object while creating your endpoint configuration with the `EndpointConfig` field in the `CreateEndpointConfig` API\. The following diagram shows the architecture and workflow of Asynchronous Inference\.

![\[Architecture diagram of Asynchronous Inference showing how a user invokes an endpoint and how the endpoint responds.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/async-architecture.png)

To invoke the endpoint, you need to place the request payload in Amazon S3 and provide a pointer to this payload as a part of the `InvokeEndpointAsync` request\. Upon invocation, SageMaker queues the request for processing and returns an identifier and output location as a response\. Upon processing, SageMaker places the result in the Amazon S3 location\. You can optionally choose to receive success or error notifications with Amazon SNS\. For more information about how to set up asynchronous notifications, see [Check prediction results](async-inference-check-predictions.md)\.

**Note**  
The presence of an asynchronous inference configuration \(`AsyncInferenceConfig`\) object in the endpoint configuration implies that the endpoint can only receive asynchronous invocations\.

## How Do I Get Started?<a name="async-inference-how-to-get-started"></a>

If you are a first\-time user of Amazon SageMaker Asynchronous Inference, we recommend that you do the following:
+ Read [Create, invoke, and update an Asynchronous Endpoint](async-inference-create-invoke-update-delete.md) for information on how to create, invoke, update, and delete an asynchronous endpoint\.
+ Explore the [Asynchronous Inference example notebook](https://github.com/aws/amazon-sagemaker-examples/blob/master/async-inference/Async-Inference-Walkthrough.ipynb) in the [aws/amazon\-sagemaker\-examples](https://github.com/aws/amazon-sagemaker-examples) GitHub repository\.

Note that if your endpoint uses any of the features listed in this [Exclusions](deployment-guardrails-exclusions.md) page, you cannot use Asynchronous Inference\.