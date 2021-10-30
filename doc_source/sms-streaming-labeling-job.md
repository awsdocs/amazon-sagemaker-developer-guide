# Ground Truth Streaming Labeling Jobs<a name="sms-streaming-labeling-job"></a>

If you want to perpetually send new data objects to Amazon SageMaker Ground Truth to be labeled, use a streaming labeling job\. Streaming labeling jobs allow you to:
+ Send new dataset objects to workers in real time using a perpetually running labeling job\. Workers continuously receive new data objects to label as long as the labeling job is active and new objects are being sent to it\.
+ Gain visibility into the number of objects that have been queued and are waiting to be labeled\. Use this information to control the flow of data objects sent to your labeling job\.
+ Receive label data for individual data objects in real time as workers finish labeling them\. 

Ground Truth streaming labeling jobs remain active until they are manually stopped or have been idle for more than 10 days\. You can intermittently send new data objects to workers while the labeling job is active\.

If you are a new user of Ground Truth streaming labeling jobs, it is recommended that you review [How It Works](#sms-streaming-how-it-works)\. 

Use [Create a Streaming Labeling Job](sms-streaming-create-job.md) to learn how to create a streaming labeling job\.

**Note**  
Ground Truth streaming labeling jobs are only supported through the SageMaker API\.

**Topics**
+ [How It Works](#sms-streaming-how-it-works)
+ [Send Data to a Streaming Labeling Job](#sms-streaming-how-it-works-send-data)
+ [Manage Labeling Requests with an Amazon SQS Queue](#sms-streaming-how-it-works-sqs)
+ [Receive Output Data from a Streaming Labeling Job](#sms-streaming-how-it-works-output-data)
+ [Duplicate Message Handling](#sms-streaming-impotency)

## How It Works<a name="sms-streaming-how-it-works"></a>

When you create a Ground Truth streaming labeling job, the job remains active until it is manually stopped, remains idle for more than 10 days, or is unable to access input data sources\. You can intermittently send new data objects to workers while it is active\. A worker can continue to receive new data objects in real time as long as the total number of tasks currently available to the worker is less than the value in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-MaxConcurrentTaskCount](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-MaxConcurrentTaskCount)\. Otherwise, the data object is sent to a queue that Ground Truth creates on your behalf in [Amazon Simple Queue Service](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) \(Amazon SQS\) for later processing\. These tasks are sent to workers as soon as the total number of tasks currently available to a worker falls below `MaxConcurrentTaskCount`\. If a data object is not sent to a worker after 14 days, it expires\. You can view the number of tasks pending in the queue and adjust the number of objects you send to the labeling job\. For example, you may decrease the speed at which you send objects to the labeling job if the backlog of pending objects moves above a threshold\. 

## Send Data to a Streaming Labeling Job<a name="sms-streaming-how-it-works-send-data"></a>

You can optionally submit input data to a streaming labeling job one time when you create the labeling job using an input manifest file\. Once the labeling job has started and the state is `InProgress`, you can submit new data objects to your labeling job in real time using your Amazon SNS input topic and Amazon S3 event notifications\. 

***Submit Data Objects When you Start the Labeling Job \(One Time\):***
+ **Use an Input Manifest File** – You can optionally specify an input manifest file Amazon S3 URI in `ManifestS3Uri` when you create the streaming labeling job\. Ground Truth sends each data object in the manifest file to workers for labeling as soon as the labeling job starts\. To learn more, see [Create a Manifest File \(Optional\)](sms-streaming-manifest.md)\.

  After you submit a request to create the streaming labeling job, its status will be `Initializing`\. Once the labeling job is active, the state changes to `InProgress` and you can start using the real\-time options to submit additional data objects for labeling\. 

***Submit Data Objects in Real Time:***
+ **Send data objects using Amazon SNS messages** – You can send Ground Truth new data objects to label by sending an Amazon SNS message\. You will send this message to an Amazon SNS input topic that you create and specify when you create your streaming labeling job\. For more information, see [Send Data Objects Using Amazon SNS](#sms-streaming-how-it-works-sns)\.
+ **Send data objects by placing them in an Amazon S3 bucket** – Each time you add a new data object to an Amazon S3 bucket, you can prompt Ground Truth to process that object for labeling\. To do this, you add an event notification to the bucket so that it notifies your Amazon SNS input topic each time a new object is added to \(or *created in*\) that bucket\. For more information, see [Send Data Objects using Amazon S3](#sms-streaming-how-it-works-s3)\. This option is not available for text\-based labeling jobs such as text classification and named entity recognition\. 
**Important**  
If you use the Amazon S3 configuration, do not use the same Amazon S3 location for your input data configuration and your output data\. You specify the S3 prefix for your output data when you create a labeling job\.

### Send Data Objects Using Amazon SNS<a name="sms-streaming-how-it-works-sns"></a>

You can send data objects to your streaming labeling job using Amazon Simple Notification Service \(Amazon SNS\)\. Amazon SNS is a web service that coordinates and manages the delivery of messages to and from *endpoints* \(for example, an email address or AWS Lambda function\)\. An Amazon SNS *topic* acts as a communication channel between two or more endpoints\. You use Amazon SNS to send, or *publish*, new data objects to the topic specified in the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) parameter `SnsTopicArn` in `InputConfig`\. The format of these messages is the same as a single line from an [input manifest file](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-data-input.html)\. 

For example, you may send a piece of text to an active text classification labeling job by publishing it to your input topic\. The message that you publish may look similar to the following:

```
{"source": "Lorem ipsum dolor sit amet"}
```

To send a new image object to an image classification labeling job, your message may look similar to the following:

```
{"source-ref": "s3://awsexamplebucket/example-image.jpg"}
```

**Note**  
You can also include custom deduplication IDs and deduplication keys in your Amazon SNS messages\. To learn more, see [ Duplicate Message Handling](#sms-streaming-impotency)\.

When Ground Truth creates your streaming labeling job, it subscribes to your Amazon SNS input topic\. 

### Send Data Objects using Amazon S3<a name="sms-streaming-how-it-works-s3"></a>

You can send one or more new data objects to a streaming labeling job by placing them in an Amazon S3 bucket that is configured with an Amazon SNS event notification\. You can set up an event to notify your Amazon SNS input topic anytime a new object is created in your bucket\. You must specify this same Amazon SNS input topic in the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) parameter `SnsTopicArn` in `InputConfig`\.

Anytime you configure an Amazon S3 bucket to send notifications to Amazon SNS, Ground Truth will publish a test event, `"s3:TestEvent"`, to ensure that the topic exists and that the owner of the Amazon S3 bucket specified has permission to publish to the specified topic\. It is recommended that you set up your Amazon S3 connection with Amazon SNS before starting a streaming labeling job\. If you do not, this test event may register as a data object and be sent to Ground Truth for labeling\. 

**Important**  
If you use the Amazon S3 configuration, do not use the same Amazon S3 location for your input data configuration and your output data\. You specify the S3 prefix for your output data when you create a labeling job\.  
For image\-based labeling jobs, Ground Truth requires all S3 buckets to have a CORS policy attached\. To learn more, see [CORS Permission Requirement](sms-cors-update.md)\.

Once you have configured your Amazon S3 bucket and created your labeling job, you can add objects to your bucket and Ground Truth either sends that object to workers or places it on your Amazon SQS queue\. 

To learn more, see [Set up Amazon S3 Bucket Event Notifications](sms-streaming-s3-setup.md)\.

**Important**  
This option is not available for text\-based labeling jobs such as text classification and named entity recognition\.

## Manage Labeling Requests with an Amazon SQS Queue<a name="sms-streaming-how-it-works-sqs"></a>

When Ground Truth creates your streaming labeling job, it creates an Amazon SQS queue in the AWS account used to create the labeling job\. The queue name is `GroundTruth-labeling_job_name` where `labeling_job_name` is the name of your labeling job, in lowercase letters\. When you send data objects to your labeling job, Ground Truth either sends the data objects directly to workers or places the task in your queue to be processed at a later time\. If a data object is not sent to a worker after 14 days, it expires and is removed from the queue\. You can setup an alarm in Amazon SQS to detect when objects expire and use this mechanism to control the volume of objects you send to your labeling job\.

**Important**  
Modifying, deleting, or sending objects directly to the Amazon SQS queue associated with your streaming labeling job may lead to job failures\. 

## Receive Output Data from a Streaming Labeling Job<a name="sms-streaming-how-it-works-output-data"></a>

Your Amazon S3 output bucket is periodically updated with new output data from your streaming labeling job\. 

Optionally, you can specify an Amazon SNS output topic\. Each time a worker submits a labeled object, a notification with the output data is sent to that topic\. You can subscribe an endpoint to your SNS output topic to receive notifications or trigger events when you receive output data from a labeling task\. Use an Amazon SNS output topic if you want to do real time chaining to another streaming job and receive an Amazon SNS notifications each time a data object is submitted by a worker\.

To learn more, see [Subscribe an Endpoint to Your Amazon SNS Output Topic](sms-create-sns-input-topic.md#sms-streaming-subscribe-output-topic)\.

## Duplicate Message Handling<a name="sms-streaming-impotency"></a>

For data objects sent in real time, Ground Truth guarantees idempotency by ensuring each unique object is only sent for labeling once, even if the input message referring to that object is received multiple times \(duplicate messages\)\. To do this, each data object sent to a streaming labeling job is assigned a *deduplication ID*, which is identified with a *deduplication key*\.

If you send your requests to label data objects directly through your Amazon SNS input topic using Amazon SNS messages, you can optionally choose a custom deduplication key and deduplication IDs for your objects\. For more information, see [Specify A Deduplication Key and ID in an Amazon SNS Message](#sms-streaming-impotency-create)\.

If you do not provide your own deduplication key, or if you use the Amazon S3 configuration to send data objects to your labeling job, Ground Truth uses one of the following for the deduplication ID:
+ For messages sent directly to your Amazon SNS input topic, Ground Truth uses the SNS message ID\. 
+ For messages that come from an Amazon S3 configuration, Ground Truth creates a deduplication ID by combining the Amazon S3 URI of the object with the [sequencer token](https://docs.aws.amazon.com/AmazonS3/latest/dev/notification-content-structure.html) in the message\.

### Specify A Deduplication Key and ID in an Amazon SNS Message<a name="sms-streaming-impotency-create"></a>

When you send a data object to your streaming labeling job using an Amazon SNS message, you have the option to specify your deduplication key and deduplication ID in one of the following ways\. In all of these scenarios, identify your deduplication key with `dataset-objectid-attribute-name`\.

**Bring Your Own Deduplication Key and ID**

Create your own deduplication key and deduplication ID by configuring your Amazon SNS message as follows\. Replace `byo-key` with your key and `UniqueId` with the deduplication ID for that data object\.

```
{
    "source-ref":"s3://bucket/prefix/object1", 
    "dataset-objectid-attribute-name":"byo-key",
    "byo-key":"UniqueId" 
}
```

Your deduplication key can be up to 140 characters\. Supported patterns include: `"^[$a-zA-Z0-9](-*[a-zA-Z0-9])*"`\.

Your deduplication ID can be up to 1,024 characters\. Supported patterns include: `^(https|s3)://([^/]+)/?(.*)$`\.

**Use an Existing Key for your Deduplication Key**

You can use an existing key in your message as the deduplication key\. When you do this, the value associated with that key is used for the deduplication ID\. 

For example, you can specify use the `source-ref` key as your deduplication key by formatting your message as follows: 

```
{
    "source-ref":"s3://bucket/prefix/object1",
    "dataset-objectid-attribute-name":"source-ref" 
}
```

In this example, Ground Truth uses `"s3://bucket/prefix/object1"` for the deduplication id\.

### Find Deduplication Key and ID in Your Output Data<a name="sms-streaming-impotency-output"></a>

You can see the deduplication key and ID in your output data\. The deduplication key is identified by `dataset-objectid-attribute-name`\.

When you use your own custom deduplication key, your output contains something similar to the following:

```
"dataset-objectid-attribute-name": "byo-key",
"byo-key": "UniqueId",
```

When you do not specify a key, you can find the deduplication ID that Ground Truth assigned to your data object as follows\. The `$label-attribute-name-object-id` parameter identifies your deduplication ID\. 

```
{
    "source-ref":"s3://bucket/prefix/object1", 
    "dataset-objectid-attribute-name":"$label-attribute-name-object-id"
    "label-attribute-name" :0,
    "label-attribute-name-metadata": {...},
    "$label-attribute-name-object-id":"<service-generated-key>"
}
```

For `<service-generated-key>`, if the data object came through an Amazon S3 configuration, Ground Truth adds a unique value used by the service and emits a new field keyed by `$sequencer` which shows the Amazon S3 sequencer used\. If object was fed to SNS directly, Ground Truth use the SNS message ID\.

**Note**  
Do not use the `$` character in your label attribute name\. 