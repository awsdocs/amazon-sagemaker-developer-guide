# Monitor Amazon SageMaker with Amazon CloudWatch<a name="monitoring-cloudwatch"></a>

You can monitor Amazon SageMaker using Amazon CloudWatch, which collects raw data and processes it into readable, near real\-time metrics\. These statistics are kept for 15 months, so that you can access historical information and gain a better perspective on how your web application or service is performing\. However, the Amazon CloudWatch console limits the search to metrics that were updated in the last 2 weeks\. This limitation ensures that the most current jobs are shown in your namespace\. To graph metrics without using a search, specify its exact name in the source view\. You can also set alarms that watch for certain thresholds, and send notifications or take actions when those thresholds are met\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

SageMaker model training jobs and endpoints write CloudWatch metrics and logs\. The following tables list the metrics and dimensions for SageMaker\.

**Endpoint Invocation Metrics** 

The `aws/sagemaker` namespace includes the following request metrics from calls to [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html)\.

Metrics are available at a 1\-minute frequency\.

For information about how long CloudWatch metrics are retained for, see [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html) in the *Amazon CloudWatch API Reference*\.


| Metric | Description | 
| --- | --- | 
| Invocation4XXErrors |  The number of `InvokeEndpoint` requests where the model returned a 4xx HTTP response code\. For each 4xx response, 1 is sent; otherwise, 0 is sent\. Units: None Valid statistics: Average, Sum  | 
| Invocation5XXErrors |  The number of `InvokeEndpoint` requests where the model returned a 5xx HTTP response code\. For each 5xx response, 1 is sent; otherwise, 0 is sent\. Units: None Valid statistics: Average, Sum  | 
| Invocations |  The `number of InvokeEndpoint` requests sent to a model endpoint\.  To get the total number of requests sent to a model endpoint, use the Sum statistic\. Units: None Valid statistics: Sum, Sample Count  | 
| InvocationsPerInstance |  The number of invocations sent to a model, normalized by `InstanceCount` in each ProductionVariant\. 1/`numberOfInstances` is sent as the value on each request, where `numberOfInstances` is the number of active instances for the ProductionVariant behind the endpoint at the time of the request\. Units: None Valid statistics: Sum  | 
| ModelLatency |  The interval of time taken by a model to respond as viewed from SageMaker\. This interval includes the local communication times taken to send the request and to fetch the response from the container of a model and the time taken to complete the inference in the container\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 
| OverheadLatency |  The interval of time added to the time taken to respond to a client request by SageMaker overheads\. This interval is measured from the time SageMaker receives the request until it returns a response to the client, minus the `ModelLatency`\. Overhead latency can vary depending on multiple factors, including request and response payload sizes, request frequency, and authentication/authorization of the request\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 

**Dimensions for Endpoint Invocation Metrics**


| Dimension | Description | 
| --- | --- | 
| EndpointName, VariantName |  Filters endpoint invocation metrics for a `ProductionVariant` of the specified endpoint and variant\.  | 

**Multi\-Model Endpoint Model Loading Metrics** 

The `aws/SageMaker` namespace includes the following model loading metrics from calls to [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html) \.

Metrics are available at a 1\-minute frequency\.

For information about how long CloudWatch metrics are retained for, see [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html) in the *Amazon CloudWatch API Reference*\.


| Metric | Description | 
| --- | --- | 
| ModelLoadingWaitTime  |  The interval of time that an invocation request has waited for the target model to be downloaded, or loaded, or both in order to perform inference\.  Units: Microseconds  Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelUnloadingTime  |  The interval of time that it took to unload the model through the container's `UnloadModel` API call\.  Units: Microseconds  Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelDownloadingTime | The interval of time that it took to download the model from Amazon Simple Storage Service \(Amazon S3\)\.Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 
| ModelLoadingTime  |  The interval of time that it took to load the model through the container's `LoadModel` API call\. Units: Microseconds  Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelCacheHit  |  The number of `InvokeEndpoint` requests sent to the multi\-model endpoint for which the model was already loaded\. The Average statistic shows the ratio of requests for which the model was already loaded\. Units: None Valid statistics: Average, Sum, Sample Count  | 

**Dimensions for Multi\-Model Endpoint Model Loading Metrics**


| Dimension | Description | 
| --- | --- | 
| EndpointName, VariantName |  Filters endpoint invocation metrics for a `ProductionVariant` of the specified endpoint and variant\.  | 

**Multi\-Model Endpoint Model Instance Metrics** 

The `/aws/sagemaker/Endpoints` namespaces include the following instance metrics from calls to [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html)\.

Metrics are available at a 1\-minute frequency\.

For information about how long CloudWatch metrics are retained for, see [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html) in the *Amazon CloudWatch API Reference*\.


| Metric | Description | 
| --- | --- | 
| LoadedModelCount  |  The number of models loaded in the containers of the multi\-model endpoint\. This metric is emitted per instance\. The Average statistic with a period of 1 minute tells you the average number of models loaded per instance\. The Sum statistic tells you the total number of models loaded across all instances in the endpoint\. The models that this metric tracks are not necessarily unique because a model might be loaded in multiple containers at the endpoint\. Units: None Valid statistics: Average, Sum, Min, Max, Sample Count  | 

**Dimensions for Multi\-Model Endpoint Model Loading Metrics**


| Dimension | Description | 
| --- | --- | 
| EndpointName, VariantName |  Filters endpoint invocation metrics for a `ProductionVariant` of the specified endpoint and variant\.  | 

**Processing Job, Training Job, Batch Transform Job, and Endpoint Instance Metrics**

The `/aws/sagemaker/ProcessingJobs`, `/aws/sagemaker/TrainingJobs`, `/aws/sagemaker/TransformJobs` and `/aws/sagemaker/Endpoints` namespaces include the following metrics for the training jobs and endpoint instances\.

Metrics are available at a 1\-minute frequency\.


| Metric | Description | 
| --- | --- | 
| CPUUtilization | The percentage of CPU units that are used by the containers on an instance\. The value can range between 0 and 100, and is multiplied by the number of CPUs\. For example, if there are four CPUs, `CPUUtilization` can range from 0% to 400%\.For processing jobs, the value is the CPU utilization of the processing container on the instance\.For training jobs, the value is the CPU utilization of the algorithm container on the instance\.For batch transform jobs, the value is the CPU utilization of the transform container on the instance\.For endpoint variants, the value is the sum of the CPU utilization of the primary and supplementary containers on the instance\. For multi\-instance, each instance reports CPU utilization metrics\. However, the default view in CloudWatch shows the average CPU utilization across all instances\. Units: Percent | 
| MemoryUtilization | The percentage of memory that is used by the containers on an instance\. This value can range between 0% and 100%\.For processing jobs, the value is the memory utilization of the processing container on the instance\.For training jobs, the value is the memory utilization of the algorithm container on the instance\.For batch transform jobs, the value is the memory utilization of the transform container on the instance\.For endpoint variants, the value is the sum of the memory utilization of the primary and supplementary containers on the instance\.Units: Percent For multi\-instance, each instance reports memory utilization metrics\. However, the default view in CloudWatch shows the average memory utilization across all instances\.  | 
| GPUUtilization | The percentage of GPU units that are used by the containers on an instance\. The value can range between 0 and 100 and is multiplied by the number of GPUs\. For example, if there are four GPUs, `GPUUtilization` can range from 0% to 400%\.For processing jobs, the value is the GPU utilization of the processing container on the instance\.For training jobs, the value is the GPU utilization of the algorithm container on the instance\.For batch transform jobs, the value is the GPU utilization of the transform container on the instance\.For endpoint variants, the value is the sum of the GPU utilization of the primary and supplementary containers on the instance\. For multi\-instance, each instance reports GPU utilization metrics\. However, the default view in CloudWatch shows the average GPU utilization across all instances\. Units: Percent | 
| GPUMemoryUtilization | The percentage of GPU memory used by the containers on an instance\. The value can range between 0 and 100 and is multiplied by the number of GPUs\. For example, if there are four GPUs, `GPUMemoryUtilization` can range from 0% to 400%\.For processing jobs, the value is the GPU memory utilization of the processing container on the instance\.For training jobs, the value is the GPU memory utilization of the algorithm container on the instance\.For batch transform jobs, the value is the GPU memory utilization of the transform container on the instance\.For endpoint variants, the value is the sum of the GPU memory utilization of the primary and supplementary containers on the instance\. For multi\-instance, each instance reports GPU memory utilization metrics\. However, the default view in CloudWatch shows the average GPU memory utilization across all instances\. Units: Percent | 
| DiskUtilization | The percentage of disk space used by the containers on an instance uses\. This value can range between 0% and 100%\. This metric is not supported for batch transform jobs\.For processing jobs, the value is the disk space utilization of the processing container on the instance\.For training jobs, the value is the disk space utilization of the algorithm container on the instance\.For endpoint variants, the value is the sum of the disk space utilization of the primary and supplementary containers on the instance\.Units: Percent For multi\-instance, each instance reports disk utilization metrics\. However, the default view in CloudWatch shows the average disk utilization across all instances\.  | 

**Dimensions for Processing Job, Training Job and Batch Transform Job Instance Metrics**


| Dimension | Description | 
| --- | --- | 
| Host |  For processing jobs, the value for this dimension has the format `[processing-job-name]/algo-[instance-number-in-cluster]`\. Use this dimension to filter instance metrics for the specified processing job and instance\. This dimension format is present only in the `/aws/sagemaker/ProcessingJobs` namespace\. For training jobs, the value for this dimension has the format `[training-job-name]/algo-[instance-number-in-cluster]`\. Use this dimension to filter instance metrics for the specified training job and instance\. This dimension format is present only in the `/aws/sagemaker/TrainingJobs` namespace\. For batch transform jobs, the value for this dimension has the format `[transform-job-name]/[instance-id]`\. Use this dimension to filter instance metrics for the specified batch transform job and instance\. This dimension format is present only in the `/aws/sagemaker/TransformJobs` namespace\.  | 

**Amazon SageMaker Ground Truth Metrics**


| Metric | Description | 
| --- | --- | 
| ActiveWorkers |  The number of workers on a private work team performing a labeling job\. Units: None Valid statistics: Max  | 
| DatasetObjectsAutoAnnotated |  The number of dataset objects auto\-annotated in a labeling job\. This metric is only emitted when automated labeling is enabled\. To view the labeling job progress, use the Max metric\. Units: None Valid statistics: Max  | 
| DatasetObjectsHumanAnnotated |  The number of dataset objects annotated by a human in a labeling job\. To view the labeling job progress, use the Max metric\. Units: None Valid statistics: Max  | 
| DatasetObjectsLabelingFailed |  The number of dataset objects that failed labeling in a labeling job\. To view the labeling job progress, use the Max metric\. Units: None Valid statistics: Max  | 
| JobsFailed |  The number of labeling jobs that failed\. To get the total number of labeling jobs that failed, use the Sum statistic\. Units: None Valid statistics: Sum, Sample Count  | 
| JobsSucceeded |  The number of labeling jobs that succeeded\. To get the total number of labeling jobs that succeeded, use the Sum statistic\. Units: None Valid statistics: Sum, Sample Count  | 
| JobsStopped |  The number of labeling jobs that were stopped\. To get the total number of labeling jobs that were stopped, use the Sum statistic\. Units: None Valid statistics: Sum, Sample Count  | 
| TasksAccepted |   The total number of tasks accepted by workers\.  Units: None  Valid statistics: Max   | 
| TasksReturned |   The total number of tasks returned by workers\.  Units: None  Valid statistics: Max   | 
| TasksSubmitted |  The number of tasks submitted/completed by a private work team\. Units: None Valid statistics: Max  | 
| TimeSpent |  Time spent on a task completed by a private work team\. Units: Seconds Valid statistics: Max  | 
| TotalDatasetObjectsLabeled |  The number of dataset objects labeled successfully in a labeling job\. To view the labeling job progress, use the Max metric\. Units: None Valid statistics: Max  | 

**Dimensions for Dataset Object Metrics**


| Dimension | Description | 
| --- | --- | 
| LabelingJobName |  Filters dataset object count metrics for a labeling job\.  | 