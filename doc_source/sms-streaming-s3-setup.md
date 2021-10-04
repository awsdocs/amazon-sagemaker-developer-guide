# Set up Amazon S3 Bucket Event Notifications<a name="sms-streaming-s3-setup"></a>

You can add an event notification to your Amazon S3 bucket using the Amazon S3 console, API, and language specific AWS SDKs, or the AWS Command Line Interface\. Set up this event to send notifications to the same Amazon SNS input topic that you specify using `SnsTopicArn` in `InputConfig` when you create a labeling job\. Do not set up event notifications using the same Amazon S3 location that you specified for `S3OutputPath` in `OutputConfig` – doing so may result in unwanted data objects being processed by Ground Truth for labeling\.

You decide the types of events that you want to send to your Amazon SNS topic\. Ground Truth creates a labeling job when you send [object creation events](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/enable-event-notifications.html#enable-event-notifications-types)\. 

The event structure sent to your Amazon SNS input topic must be a JSON message formatted using the same structure found in [Event message structure](https://docs.aws.amazon.com/AmazonS3/latest/dev/notification-content-structure.html)\.

To see examples of how you can set up an event notification for your Amazon S3 bucket using the Amazon S3 console, AWS SDK for \.NET, and AWS SDK for Java, follow this walkthrough, [Walkthrough: Configure a bucket for notifications \(SNS topic or SQS queue\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/ways-to-add-notification-config-to-bucket.html) in the Amazon Simple Storage Service User Guide\.