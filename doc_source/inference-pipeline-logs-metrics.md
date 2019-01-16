# Logs and Metrics<a name="inference-pipeline-logs-metrics"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon SageMaker resources\. For information about the monitoring tools that Amazon SageMaker provides to watch your resources, report when something is wrong, and take automatic actions when appropriate, see [Monitoring Amazon SageMaker](monitoring-overview.md)\.

## Metrics<a name="inference-pipeline-metrics"></a>

You can monitor multi\-container models in Amazon SageMaker Inference Pipelines using Amazon CloudWatch, which collects raw data and processes it into readable, near real\-time metrics\. Amazon SageMaker model training jobs and endpoints write Amazon CloudWatch metrics and logs\. For more information on monitoring with Amazon CloudWatch, see [Monitoring Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md) \. The following tables list the metrics and dimensions for Amazon SageMaker\.

**Endpoint Invocation Metrics** 

The `AWS/SageMaker` namespace includes the following request metrics from calls to [InvokeEndpoint](API_runtime_InvokeEndpoint.md) \.

Metrics are available at a 1\-minute frequency\.


| Metric | Description | 
| --- | --- | 
| Invocation4XXErrors |  The number of `InvokeEndpoint` requests where the model returned a 4xx HTTP response code\. For each 4xx response, 1 is sent; otherwise, 0 is sent\. Units: None Valid statistics: Average, Sum  | 
| Invocation5XXErrors |  The number of `InvokeEndpoint` requests where the model returned a 5xx HTTP response code\. For each 5xx response, 1 is sent; otherwise, 0 is sent\. Units: None Valid statistics: Average, Sum  | 
| Invocations |  The `number of InvokeEndpoint` requests sent to a model endpoint\.  To get the total number of requests sent to a model endpoint, use the Sum statistic\. Units: None Valid statistics: Sum, Sample Count  | 
| InvocationsPerInstance |  The number of invocations sent to a model, normalized by `InstanceCount` in each ProductionVariant\. 1/`numberOfInstances` is sent as the value on each request, where `numberOfInstances` is the number of active instances for the ProductionVariant behind the endpoint at the time of the request\. Units: None Valid statistics: Sum  | 
| ModelLatency | The interval of time taken by the model or models to respond as viewed from Amazon SageMaker\. This interval includes the local communication times taken to send the request and to fetch the response from the container of a model and the time taken to complete the inference in the container\. For an inference pipeline endpoint, the ModelLatency is the total interval of time taken by all the containers\.Units: MicrosecondsValid statistics: Average, Sum, Min, Max, Sample Count | 
| OverheadLatency |  The interval of time added to the time taken to respond to a client request by Amazon SageMaker overheads\. This interval is measured from the time Amazon SageMaker receives the request until it returns a response to the client, minus the `ModelLatency`\. Overhead latency can vary depending on multiple factors, including request and response payload sizes, request frequency, and authentication or authorization of the request\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 
| ContainerLatency | The interval of time taken by an Inference Pipelines container to respond as viewed from Amazon SageMaker\. This interval includes the local communication times taken to send the request and to fetch the response from the container of a model and the time taken to complete the inference in the container\.Units: MicrosecondsValid statistics: Average, Sum, Min, Max, Sample Count | 

**Dimensions for Endpoint Invocation Metrics**


| Dimension | Description | 
| --- | --- | 
| EndpointName, VariantName, ContainerName |  Filters endpoint invocation metrics for a `ProductionVariant` of the specified endpoint and variant\.  | 

For an inference pipeline endpoint, per\-container latency metrics in your account show as **Endpoint Container Metrics** and **Endpoint Variant Metrics** under the **SageMaker** namespace\. `ContainerLatency` shows up only if it's an inferences pipeline\.

![\[An image of the inference pipeline endpoint per-container latency metrics dashboard.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pipeline-endpoint-metrics.png)

The per\-endpoint, per\-container latency metrics display the **ContainerName**, **EndpointName**, **VariantName**, and **Metric Name**\.

![\[An image of the per-endpoint latency metrics .\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pipeline-endpoint-metrics-details.png)

**Training Job, Batch Transform Job, and Endpoint Instance Metrics**

The `/aws/sagemaker/TrainingJobs`, `/aws/sagemaker/TransformJobs`, and `/aws/sagemaker/Endpoints` namespaces include the following metrics for the training jobs and endpoint instances\.

Metrics are available at a 1\-minute frequency\.


| Metric | Description | 
| --- | --- | 
| CPUUtilization |  The percentage of CPU units that are used by the containers on an instance\. The value can range between 0 and 100, and is multiplied by the number of CPUs\. For example, if there are four CPUs, `CPUUtilization` can range from 0% to 400%\. For training jobs, the value is the CPU utilization of the algorithm container on the instance\. For batch transform jobs, the value is the CPU utilization of the transform container on the instance\. For multi\-container models, the value is sum of CPU utilization by all containers\. For endpoint variants, the value is the sum of the CPU utilization of all provided containers on the instance\. Units: Percent  | 
| MemoryUtilizaton | The percentage of memory that is used by the containers on an instance\. This value can range between 0% and 100%\.For training jobs, the value is the memory utilization of the algorithm container on the instance\.For batch transform jobs, the value is the memory utilization of the transform container on the instance\.For multi\-container models, the value is sum of memory utilization by all containers\.For endpoint variants, the value is the sum of the memory utilization of all provided containers on the instance\.Units: Percent | 
| GPUUtilization |  The percentage of GPU units that are used by the containers on an instance\. The value can range between 0 and 100 and is multiplied by the number of GPUs\. For example, if there are four GPUs, `GPUUtilization` can range from 0% to 400%\. For training jobs, the value is the GPU utilization of the algorithm container on the instance\. For batch transform jobs, the value is the GPU utilization of the transform container on the instance\. For multi\-container models, the value is sum of GPU utilization by all containers\. For endpoint variants, the value is the sum of the GPU utilization of the all provided containers on the instance\. Units: Percent  | 
| GPUMemoryUtilization |  The percentage of GPU memory used by the containers on an instance\. The value can range between 0 and 100 and is multiplied by the number of GPUs\. For example, if there are four GPUs, `GPUMemoryUtilization` can range from 0% to 400%\. For training jobs, the value is the GPU memory utilization of the algorithm container on the instance\. For batch transform jobs, the value is the GPU memory utilization of the transform container on the instance\. For multi\-container models, the value is sum of GPU utilization by all containers\. For endpoint variants, the value is the sum of the GPU memory utilization of the all provided containers on the instance\. Units: Percent  | 
| DiskUtilization |  The percentage of disk space used by the containers on an instance uses\. This value can range between 0% and 100%\. This metric is not supported for batch transform jobs\. For training jobs, the value is the disk space utilization of the algorithm container on the instance\. For endpoint variants, the value is the sum of the disk space utilization of all the provided containers on the instance\. Units: Percent  | 

**Dimensions for Training Job, Batch Transform Job, and Endpoint Instance Metrics**


| Dimension | Description | 
| --- | --- | 
| Host |  For training jobs, the value for this dimension has the format `[training-job-name]/algo-[instance-number-in-cluster]`\. Use this dimension to filter instance metrics for the specified training job and instance\. This dimension format is present only in the `/aws/sagemaker/TrainingJobs` namespace\. For batch transform jobs, the value for this dimension has the format `[transform-job-name]/[instance-id]`\. Use this dimension to filter instance metrics for the specified batch transform job and instance\. This dimension format is present only in the `/aws/sagemaker/TransformJobs` namespace\. For endpoints, the value for this dimension has the format `[endpoint-name]/[ production-variant-name ]/[instance-id]`\. Use this dimension to filter instance metrics for the specified endpoint, variant, and instance\. This dimension format is present only in the `/aws/sagemaker/Endpoints` namespace\.  | 

To help you debug your training jobs, endpoints, and notebook instance lifecycle configurations, anything an algorithm container, a model container, or a notebook instance lifecycle configuration sends to `stdout` or `stderr` is also sent to Amazon CloudWatch Logs\. In addition to debugging, you can use these for progress analysis\.

## Logs<a name="inference-pipeline-logs"></a>

The following table lists all of the logs provided by Amazon SageMaker\.

**Logs**

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipeline-logs-metrics.html)

**Note**  
1\. The `/aws/sagemaker/NotebookInstances` log group is created when you create a notebook instance with a lifecycle configuration\. For more information, see [Step 2\.1: \(Optional\) Customize a Notebook Instance ](notebook-lifecycle-config.md)\.  
2\. For Inference Pipelines, if you don't provide container names, the platform uses \*\*container\-1, container\-2\*\*, and so on, corresponding to the order provided in the Amazon SageMaker model\.

For more information on Amazon SageMaker logging, see [Logging Amazon SageMaker with Amazon CloudWatch](logging-cloudwatch.md)\. 