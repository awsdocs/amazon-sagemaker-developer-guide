# Create a Labeling Job<a name="sms-create-labeling-job"></a>

You can create a labeling job in the Amazon SageMaker console and by using an AWS SDK in your preferred language to run `CreateLabelingJob`\. After a labeling job has been created, you can track worker metrics \(for private workforces\) and your labeling job status using [CloudWatch](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-monitor-cloud-watch.html)\.

Before you create a labeling job it is recommended that you review the following pages, as applicable:
+ You can specify our input data using an automatic data setup in the console, or an input manifest file in either the console or when using `CreateLabelingJob` API\. For automated data setup, see [Automated Data Setup](sms-console-create-manifest-file.md)\. To learn how to create an input manifest file, see [Use an Input Manifest File](sms-input-data-input-manifest.md)\.
+ Review labeling job input data quotas: [Input Data Quotas](input-data-limits.md)\.

After you have chosen your task type, use the topics on this page to learn how to create a labeling job\.

If you are a new Ground Truth user, we recommend that you start by walking through the demo in [Getting started](sms-getting-started.md)\.

**Important**  
Ground Truth requires all S3 buckets that contain labeling job input image data to have a CORS policy attached\. To learn more, see [CORS Permission Requirement](sms-cors-update.md)\.

**Topics**
+ [Built\-in Task Types](sms-task-types.md)
+ [Creating Instruction Pages](sms-creating-instruction-pages.md)
+ [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md)
+ [Create a Labeling Job \(API\)](sms-create-labeling-job-api.md)
+ [Create a Streaming Labeling Job](sms-streaming-create-job.md)
+ [Create a Labeling Category Configuration File with Label Category and Frame Attributes](sms-label-cat-config-attributes.md)