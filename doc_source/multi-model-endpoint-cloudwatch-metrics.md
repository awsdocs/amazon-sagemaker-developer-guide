# CloudWatch Metrics for Multi\-Model Endpoint Deployments<a name="multi-model-endpoint-cloudwatch-metrics"></a>

Amazon SageMaker provides metrics for endpoints so you can monitor the cache hit rate, the number of models loaded and the model wait times for loading, downloading, and uploading at a multi\-model endpoint\. Some of the metrics are different for CPU and GPU backed multi\-model endpoints, so the following sections describe the Amazon CloudWatch metrics that you can use for each type of multi\-model endpoint\.

For more information about the metrics, see **Multi\-Model Endpoint Model Loading Metrics** and **Multi\-Model Endpoint Model Instance Metrics** in [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md)\. Per\-model metrics aren't supported\. 

## CloudWatch metrics for CPU backed multi\-model endpoints<a name="multi-model-endpoint-cloudwatch-metrics-cpu"></a>

You can monitor the following metrics on CPU backed multi\-model endpoints\.

The `AWS/SageMaker` namespace includes the following model loading metrics from calls to [ InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html)\.

Metrics are available at a 1\-minute frequency\.

For information about how long CloudWatch metrics are retained for, see [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html) in the *Amazon CloudWatch API Reference*\.

**Multi\-Model Endpoint Model Loading Metrics**


| Metric | Description | 
| --- | --- | 
| ModelLoadingWaitTime  |  The interval of time that an invocation request has waited for the target model to be downloaded, or loaded, or both in order to perform inference\.  Units: Microseconds  Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelUnloadingTime  |  The interval of time that it took to unload the model through the container's `UnloadModel` API call\.  Units: Microseconds  Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelDownloadingTime |  The interval of time that it took to download the model from Amazon Simple Storage Service \(Amazon S3\)\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelLoadingTime  |  The interval of time that it took to load the model through the container's `LoadModel` API call\. Units: Microseconds  Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelCacheHit  |  The number of `InvokeEndpoint` requests sent to the multi\-model endpoint for which the model was already loaded\. The Average statistic shows the ratio of requests for which the model was already loaded\. Units: None Valid statistics: Average, Sum, Sample Count  | 

**Dimensions for Multi\-Model Endpoint Model Loading Metrics**


| Dimension | Description | 
| --- | --- | 
| EndpointName, VariantName |  Filters endpoint invocation metrics for a `ProductionVariant` of the specified endpoint and variant\.  | 

The `/aws/sagemaker/Endpoints` namespaces include the following instance metrics from calls to [ InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html)\.

Metrics are available at a 1\-minute frequency\.

For information about how long CloudWatch metrics are retained for, see [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html) in the *Amazon CloudWatch API Reference*\.

**Multi\-Model Endpoint Model Instance Metrics**


| Metric | Description | 
| --- | --- | 
| LoadedModelCount  |  The number of models loaded in the containers of the multi\-model endpoint\. This metric is emitted per instance\. The Average statistic with a period of 1 minute tells you the average number of models loaded per instance\. The Sum statistic tells you the total number of models loaded across all instances in the endpoint\. The models that this metric tracks are not necessarily unique because a model might be loaded in multiple containers at the endpoint\. Units: None Valid statistics: Average, Sum, Min, Max, Sample Count  | 
| CPUUtilization  |  The sum of each individual CPU core's utilization\. The CPU utilization of each core range is 0–100\. For example, if there are four CPUs, the `CPUUtilization` range is 0%–400%\. For endpoint variants, the value is the sum of the CPU utilization of the primary and supplementary containers on the instance\. Units: Percent  | 
| MemoryUtilization |  The percentage of memory that is used by the containers on an instance\. This value range is 0%–100%\. For endpoint variants, the value is the sum of the memory utilization of the primary and supplementary containers on the instance\. Units: Percent  | 
| DiskUtilization |  The percentage of disk space used by the containers on an instance\. This value range is 0%–100%\. For endpoint variants, the value is the sum of the disk space utilization of the primary and supplementary containers on the instance\. Units: Percent  | 

## CloudWatch metrics for GPU multi\-model endpoint deployments<a name="multi-model-endpoint-cloudwatch-metrics-gpu"></a>

You can monitor the following metrics on GPU backed multi\-model endpoints\.

The `AWS/SageMaker` namespace includes the following model loading metrics from calls to [ InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html)\.

Metrics are available at a 1\-minute frequency\.

For information about how long CloudWatch metrics are retained for, see [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html) in the *Amazon CloudWatch API Reference*\.

**Multi\-Model Endpoint Model Loading Metrics**


| Metric | Description | 
| --- | --- | 
| ModelLoadingWaitTime  |  The interval of time that an invocation request has waited for the target model to be downloaded, or loaded, or both in order to perform inference\.  Units: Microseconds  Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelUnloadingTime  |  The interval of time that it took to unload the model through the container's `UnloadModel` API call\.  Units: Microseconds  Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelDownloadingTime |  The interval of time that it took to download the model from Amazon Simple Storage Service \(Amazon S3\)\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelLoadingTime  |  The interval of time that it took to load the model through the container's `LoadModel` API call\. Units: Microseconds  Valid statistics: Average, Sum, Min, Max, Sample Count   | 
| ModelCacheHit  |  The number of `InvokeEndpoint` requests sent to the multi\-model endpoint for which the model was already loaded\. The Average statistic shows the ratio of requests for which the model was already loaded\. Units: None Valid statistics: Average, Sum, Sample Count  | 

**Dimensions for Multi\-Model Endpoint Model Loading Metrics**


| Dimension | Description | 
| --- | --- | 
| EndpointName, VariantName |  Filters endpoint invocation metrics for a `ProductionVariant` of the specified endpoint and variant\.  | 

The `/aws/sagemaker/Endpoints` namespaces include the following instance metrics from calls to [ InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html)\.

Metrics are available at a 1\-minute frequency\.

For information about how long CloudWatch metrics are retained for, see [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html) in the *Amazon CloudWatch API Reference*\.

**Multi\-Model Endpoint Model Instance Metrics**


| Metric | Description | 
| --- | --- | 
| LoadedModelCount  |  The number of models loaded in the containers of the multi\-model endpoint\. This metric is emitted per instance\. The Average statistic with a period of 1 minute tells you the average number of models loaded per instance\. The Sum statistic tells you the total number of models loaded across all instances in the endpoint\. The models that this metric tracks are not necessarily unique because a model might be loaded in multiple containers at the endpoint\. Units: None Valid statistics: Average, Sum, Min, Max, Sample Count  | 
| CPUUtilization  |  The sum of each individual CPU core's utilization\. The CPU utilization of each core range is 0‐100\. For example, if there are four CPUs, the `CPUUtilization` range is 0%–400%\. For endpoint variants, the value is the sum of the CPU utilization of the primary and supplementary containers on the instance\. Units: Percent  | 
| MemoryUtilization |  The percentage of memory that is used by the containers on an instance\. This value range is 0%‐100%\. For endpoint variants, the value is the sum of the memory utilization of the primary and supplementary containers on the instance\. Units: Percent  | 
| GPUUtilization |  The percentage of GPU units that are used by the containers on an instance\. The value can range betweenrange is 0‐100 and is multiplied by the number of GPUs\. For example, if there are four GPUs, the `GPUUtilization` range is 0%–400%\. For endpoint variants, the value is the sum of the GPU utilization of the primary and supplementary containers on the instance\. Units: Percent  | 
| GPUMemoryUtilization |  The percentage of GPU memory used by the containers on an instance\. The value range is 0‐100 and is multiplied by the number of GPUs\. For example, if there are four GPUs, the `GPUMemoryUtilization` range is 0%‐400%\. For endpoint variants, the value is the sum of the GPU memory utilization of the primary and supplementary containers on the instance\. Units: Percent  | 
| DiskUtilization |  The percentage of disk space used by the containers on an instance\. This value range is 0%–100%\. For endpoint variants, the value is the sum of the disk space utilization of the primary and supplementary containers on the instance\. Units: Percent  | 