# Bring Your Own Containers<a name="model-monitor-byoc-containers"></a>

Amazon SageMaker Model Monitor provides a prebuilt container with ability to analyze the data captured from Endpoints for tabular datasets\. If you would like to bring your own container, Model Monitor provides extension points which you can leverage\.

Under the hood, when you create a `MonitoringSchedule`, Amazon SageMaker Model Monitor ultimately kicks off processing jobs\. Hence the container needs to be aware of the processing job contract documented in the [Build Your Own Processing Container \(Advanced Scenario\)](build-your-own-processing-container.md) topic\. Note that Amazon SageMaker Model Monitor kicks off the processing job on your behalf per the schedule\. While invoking, Model Monitor sets up additional environment variables for you so that your container has enough context to process the data for that particular execution of the scheduled monitoring\. For additional information on container inputs, see the [Container Contract Inputs](model-monitor-byoc-contract-inputs.md)\.

In the container, using the above environment variables/context, you can now analyze the dataset for the current period in your custom code\. Once this analysis is completed, you can chose to emit your reports to be uploaded to S3\. The reports that the pre\-built container generates are documented in [Container Contract Outputs](model-monitor-byoc-contract-outputs.md)\. If you would like the visualization of the reports to work in SageMaker Studio, you should follow the same format\. You can also choose to emit completely custom reports\.

You also have an option to emit CloudWatch metrics from the container by following the instructions in https://docs\.aws\.amazon\.com/sagemaker/latest/dg/model\-monitor\-byoc\-cloudwatch\.html\.

**Topics**
+ [Container Contract Inputs](model-monitor-byoc-contract-inputs.md)
+ [Container Contract Outputs](model-monitor-byoc-contract-outputs.md)
+ [CloudWatch Metrics](model-monitor-byoc-cloudwatch.md)