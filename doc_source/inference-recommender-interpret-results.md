# Interpret recommendation results<a name="inference-recommender-interpret-results"></a>

Each Inference Recommender job result includes `InstanceType`, `InitialInstanceCount`, and `EnvironmentParameters`, which are tuned environment variable parameters for your container to improve its latency and throughput\. The results also include performance and cost metrics such as `MaxInvocations`, `ModelLatency`, `CostPerHour`, and `CostPerInference`\. 

In the table below we provide a description of these metrics\. These metrics can help you narrow down your search for the best endpoint configuration that suits your use case\. For example, if your motivation is overall price performance with an emphasis on throughput, then you should focus on `CostPerInference`\. 


| Metric | Description | Use case | 
| --- | --- | --- | 
|  `ModelLatency`  |  The interval of time taken by a model to respond as viewed from SageMaker\. This interval includes the local communication times taken to send the request and to fetch the response from the container of a model and the time taken to complete the inference in the container\. Units: Milliseconds  | Latency sensitive workloads such as ad serving and medical diagnosis | 
|  `MaximumInvocations`  |  The maximum number of `InvokeEndpoint` requests sent to a model endpoint in a minute\. Units: None  | Throughput\-focused workloads such as video processing or batch inference | 
|  `CostPerHour`  |  The estimated cost per hour for your real\-time endpoint\. Units: US Dollars  | Cost sensitive workloads with no latency deadlines | 
|  `CostPerInference`  |  The estimated cost per inference call for your real\-time endpoint\. Units: US Dollars  | Maximize overall price performance with a focus on throughput | 

In some cases you might want to explore other [SageMaker Endpoint Invocation metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html#cloudwatch-metrics-endpoint-invocation) such as `CPUUtilization`\. Every Inference Recommender job result includes the names of endpoints spun up during the load test\. You can use CloudWatch to review the logs for these endpoints even after they’ve been deleted\.

The following image is an example of CloudWatch metrics and charts you can review for a single endpoint from your recommendation result\. This recommendation result is from a Default job\. The way to interpret the scalar values from the recommendation results is that they are based on the time point when the Invocations graph first begins to level out\. For example, the `ModelLatency` value reported is at the beginning of the plateau around `03:00:31`\.

![\[Charts for the following CloudWatch metrics: Invocations, ModelLatency, OverheadLatency, CPUUtilization, MemoryUtilization, DiskUtilization, Invocation4XXErrors, Invocation5XXErrors, and InvocationsPerInstance.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/inference-recommender-cw-metrics.png)

For full descriptions of the CloudWatch metrics used in the preceding charts, see [SageMaker Endpoint Invocation metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html#cloudwatch-metrics-endpoint-invocation)\.

See the [Amazon SageMaker Inference Recommender \- CloudWatch Metrics](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-inference-recommender/tensorflow-cloudwatch/tf-cloudwatch-inference-recommender.ipynb) Jupyter notebook in the [amazon\-sagemaker\-examples](https://github.com/aws/amazon-sagemaker-examples) Github repository for an example of how to use the AWS SDK for Python \(Boto3\) to explore CloudWatch metrics for your endpoints\.