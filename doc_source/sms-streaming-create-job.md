# Create a Streaming Labeling Job<a name="sms-streaming-create-job"></a>

To create a streaming labeling job, you must create two Amazon SNS topics: and *input topic* and an *output topic* and specify these topics in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) parameters `InputConfig` and `OutputConfig` respectively using `SnsDataSource`\.

**Important**  
If you are a new user of Ground Truth streaming labeling jobs, it is recommended that you review [Ground Truth Streaming Labeling Jobs](sms-streaming-labeling-job.md) before creating a streaming labeling job\.

Use the following sections to create the resources that you need and can use to create a streaming labeling job:
+ Learn how to create SNS topics with the permissions required for Ground Truth streaming labeling jobs by following the steps in [Create Amazon SNS Input and Output Topics](sms-create-sns-input-topic.md)\. Your SNS topics must be created in the same AWS Region as your labeling job\. 
+ See [Subscribe an Endpoint to Your Amazon SNS Output Topic](sms-create-sns-input-topic.md#sms-streaming-subscribe-output-topic) to learn how to set up an endpoint to receive labeling task output data at a specified endpoint each time a labeling task is completed\.
+ To learn how to configure your Amazon S3 bucket to send notifications to your Amazon SNS input topic, see [Set up Amazon S3 Bucket Event Notifications](sms-streaming-s3-setup.md)\.
+ Optionally, add data objects that you want to have labeled as soon as the labeling job starts to your input manifest\. For more information, see [Create a Manifest File \(Optional\)](sms-streaming-manifest.md)\.
+ There are other resources required to create a labeling job, such as an IAM role, Amazon S3 bucket, a worker task template and label categories\. These are described in the Ground Truth documentation on creating a labeling job\. For more information, see [Create a Labeling Job](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-create-labeling-job.html)\. 
**Important**  
When you create a labeling job you must provide an IAM execution role\. Attach the AWS managed policy **AmazonSageMakerGroundTruthExecution** to this role to ensure it has required permissions to execute your labeling job\. 

When you submit a request to create a streaming labeling job, the state of your labeling job is `Initializing`\. Once the labeling job is active, the state changes to `InProgress`\. Do not send new data objects to your labeling job or attempt to stop your labeling job while it is in the `Initializing` state\. Once the state changes to `InProgress`, you can start sending new data objects using Amazon SNS and the Amazon S3 configuration\. 

**Topics**
+ [Create Amazon SNS Input and Output Topics](sms-create-sns-input-topic.md)
+ [Set up Amazon S3 Bucket Event Notifications](sms-streaming-s3-setup.md)
+ [Create a Manifest File \(Optional\)](sms-streaming-manifest.md)
+ [Example: Use SageMaker API To Create Streaming Labeling Job](sms-streaming-create-labeling-job-api.md)
+ [Stop a Streaming Labeling Job](sms-streaming-stop-labeling-job.md)