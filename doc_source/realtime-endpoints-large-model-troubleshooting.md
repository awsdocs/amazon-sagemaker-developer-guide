# Large model inference troubleshooting<a name="realtime-endpoints-large-model-troubleshooting"></a>

 If you run into any problem or error, you can try to use the following list to troubleshoot\. If the problem persists, contact us at [LMI\-DLC\-feedback@amazon\.com](mailto:LMI-DLC-feedback@amazon.com)\. 

**Topics**
+ [Downloading a model to my instance takes a long time](#realtime-endpoints-large-model-troubleshooting-1)
+ [Inference latency or throughput performance is poor](#realtime-endpoints-large-model-troubleshooting-2)
+ [SageMaker endpoint failed to start with a timeout or out of memory error](#realtime-endpoints-large-model-troubleshooting-3)

## Downloading a model to my instance takes a long time<a name="realtime-endpoints-large-model-troubleshooting-1"></a>

 Consider using `option.s3url` in your `serving.properties` configuration file\. This downloading method uses `s5cmd` and speeds up data downloading\. Make sure that the container has permission to access the specified Amazon S3 bucket\. 

## Inference latency or throughput performance is poor<a name="realtime-endpoints-large-model-troubleshooting-2"></a>

 Even with model parallelism, large models can still have poor performance such as high latency or low throughput\. First, if your model is supported, consider using the DeepSpeed engine that includes optimized kernels for inference\. This can improve performance by up to 3 times\. Next, consider using a more powerful instance type, such as a `p4d.24xlarge` instance\. Finally, consider using a smaller model\. To achieve the best performance and lowest cost, try to find the smallest model that meets the accuracy bar for your use case, as more parameters can result in higher latency, lower throughput, or higher cost\. Consider fine\-tuning a smaller model to improve accuracy\. 

## SageMaker endpoint failed to start with a timeout or out of memory error<a name="realtime-endpoints-large-model-troubleshooting-3"></a>

 Large models can require more time to load to memory\. You must adjust default endpoint configurations to handle this incremental time\. If an endpoint fails a health check or is out of memory, check that you have configured your endpoint appropriately\. For example, if Amazon CloudWatch Logs indicate a health check timeout, you should increase the `ContainerStartupHealthCheckTimeoutInSeconds` parameter of `ProductionVariants` during the `create_endpoint_config` step of hosting on SageMaker\. 

 For more information on these parameters, see [SageMaker endpoint parameters for large model inference](realtime-endpoints-large-model-hosting.md)\. 