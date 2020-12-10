# Bring Your Own Containers<a name="model-monitor-byoc-containers"></a>

Amazon SageMaker Model Monitor provides a prebuilt container with ability to analyze the data captured from endpoints for tabular datasets\. If you would like to bring your own container, Model Monitor provides extension points which you can leverage\.

Under the hood, when you create a `MonitoringSchedule`, Model Monitor ultimately kicks off processing jobs\. Hence the container needs to be aware of the processing job contract documented in the [Build Your Own Processing Container \(Advanced Scenario\)](build-your-own-processing-container.md) topic\. Note that Model Monitor kicks off the processing job on your behalf per the schedule\. While invoking, Model Monitor sets up additional environment variables for you so that your container has enough context to process the data for that particular execution of the scheduled monitoring\. For additional information on container inputs, see the [Container Contract Inputs](model-monitor-byoc-contract-inputs.md)\.

In the container, using the above environment variables/context, you can now analyze the dataset for the current period in your custom code\. After this analysis is complete, you can chose to emit your reports to be uploaded to an S3 bucket\. The reports that the prebuilt container generates are documented in [Container Contract Outputs](model-monitor-byoc-contract-outputs.md)\. If you would like the visualization of the reports to work in SageMaker Studio, you should follow the same format\. You can also choose to emit completely custom reports\.

You also emit CloudWatch metrics from the container by following the instructions in [CloudWatch Metrics for Bring Your Own Containers](model-monitor-byoc-cloudwatch.md)\.

**Topics**
+ [Container Contract Inputs](model-monitor-byoc-contract-inputs.md)
+ [Container Contract Outputs](model-monitor-byoc-contract-outputs.md)
+ [CloudWatch Metrics for Bring Your Own Containers](model-monitor-byoc-cloudwatch.md)