# Monitor Asynchronous Endpoint<a name="async-inference-monitor"></a>

You can monitor SageMaker using Amazon CloudWatch, which collects raw data and processes it into readable, near real\-time metrics\. With Amazon CloudWatch, you can access historical information and gain a better perspective on how your web application or service is performing\. For more information about Amazon CloudWatch, see [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)

## Monitoring with CloudWatch<a name="async-inference-monitor-cloudwatch"></a>

The metrics below are an exhaustive list of metrics for asynchronous endpoints\. Any metric not listed below is not published if the endpoint is enabled for asynchronous inference\. Such metrics include \(but are not limited to\):
+ OverheadLatency
+ Invocations
+ InvocationsPerInstance

### Common Endpoint Metrics<a name="async-inference-monitor-cloudwatch-common"></a>

These metrics are the same as the metrics published for real\-time endpoints today\. For more information about other metrics in Amazon CloudWatch, see [Monitor SageMaker with Amazon CloudWatch](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html)\.


| Metric Name | Description | Unit/Stats | 
| --- | --- | --- | 
| `Invocation4XXErrors` | The number of requests where the model returned a 4xx HTTP response code\. For each 4xx response, 1 is sent; otherwise, 0 is sent\. | Units: NoneValid statistics: Average, Sum | 
| `Invocation5XXErrors` | The number of InvokeEndpoint requests where the model returned a 5xx HTTP response code\. For each 5xx response, 1 is sent; otherwise, 0 is sent\. | Units: NoneValid statistics: Average, Sum | 
| `ModelLatency` | The interval of time taken by a model to respond as viewed from SageMaker\. This interval includes the local communication times taken to send the request and to fetch the response from the container of a model and the time taken to complete the inference in the container\. | Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count | 

### Asynchronous Inference Endpoint Metrics<a name="async-inference-monitor-cloudwatch-async"></a>

These metrics are published for endpoints enabled for asynchronous inference\. The following metrics are published with the `EndpointName` dimension:


| Metric Name | Description | Unit/Stats | 
| --- | --- | --- | 
| `ApproximateBacklogSize` | The number of items in the queue for an endpoint that are currently being processed or yet to be processed\. | Units: Count Valid statistics: Average, Max, Min  | 
| `ApproximateBacklogSizePerInstance` | Number of items in the queue divided by the number of instances behind an endpoint\. This metric is primarily used for setting up application autoscaling for an async\-enabled endpoint\. | Units: CountValid statistics: Average, Max, Min | 
| `ApproximateAgeOfOldestRequest` | Age of the oldest request in the queue\. | Units: SecondsValid statistics: Average, Max, Min | 

The following metrics are published with the `EndpointName` and `VariantName` dimensions:


| Metric Name | Description | Unit/Stats | 
| --- | --- | --- | 
| `RequestDownloadFailures` | When an inference failure occurs due to an issue downloading the request from Amazon S3\. | Units: CountValid statistics: Sum | 
| `ResponseUploadFailures` | When an inference failure occurs due to an issue uploading the response to Amazon S3\. | Units: CountValid statistics: Sum | 
| `NotificationFailures` | When an issue occurs publishing notifications\. | Units: CountValid statistics: Sum | 
| `RequestDownloadLatency` | Total time to download the request payload\. | Units: MicrosecondsValid statistics: Average, Sum, Min, Max, Sample Count | 
| `ResponseUploadLatency` | Total time to upload the response payload\. | Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count | 
| `ExpiredRequests` | Number of requests in the queue that fail due to reaching their specified request TTL\. | Units: CountValid statistics: Sum | 
| `InvocationFailures` | If an invocation fails for any reason\. | Units: CountValid statistics: Sum | 
| `InvocationsProcesssed` | Number of async invocations processed by the endpoint\. | Units: CountValid statistics: Sum | 
| `TimeInBacklog` | Total time the request was queued before being processed\. This does not include the actual processing time \(i\.e\. downloading time, uploading time, model latency\)\. | Units: MicrosecondsValid statistics: Average, Sum, Min, Max, Sample Count | 
| `TotalProcessingTime` | Time the inference request was recieved by SageMaker to the time the request finished processing\. This includes time in backlog and time to upload and send response notifications, if any\. | Units: MillisecondsValid statistics: Average, Sum, Min, Max, Sample Count | 

Amazon SageMaker Asynchronous Inference also includes host\-level metrics\. For information on host\-level metrics, see [SageMaker Jobs and Endpoint Metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html#cloudwatch-metrics-jobs)\.

## Logs<a name="async-inference-monitor-logs"></a>

In addition to the [Model container logs](https://docs.aws.amazon.com/sagemaker/latest/dg/logging-cloudwatch.html) that are published to Amazon CloudWatch in your account, you also get a new platform log for tracing and debugging inference requests\.

The new logs are published under the Endpoint Log Group:

```
/aws/sagemaker/Endpoints/[EndpointName]
```

The log stream name consists of: 

```
[production-variant-name]/[instance-id]/data-log.
```

Log lines contain the request’s inference ID so that errors can be easily mapped to a particular request\.