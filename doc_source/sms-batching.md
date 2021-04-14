# Control the Flow of Data Objects Sent to Workers<a name="sms-batching"></a>

Depending on the type of labeling job you create, Amazon SageMaker Ground Truth sends data objects to workers in batches or in a streaming fashion\. You can control the flow of data objects to workers in the following ways:
+ For both types of labeling jobs, you can use `MaxConcurrentTaskCount` to control the total number of data objects available to all workers at a given point in time when the labeling job is running\.
+ For streaming labeling jobs, you can control the flow of data objects to workers by monitoring and controlling the number of data objects sent to the Amazon SQS associated with your labeling job\. 

Use the following sections to learn more about these options\. To learn more about streaming labeling jobs, see [Ground Truth Streaming Labeling Jobs](sms-streaming-labeling-job.md)\.

**Topics**
+ [Use MaxConcurrentTaskCount to Control the Flow of Data Objects](#sms-batching-maxconcurrenttaskcount)
+ [Use Amazon SQS to Control the Flow of Data Objects to Streaming Labeling Jobs](#sms-batching-streaming-sqs)

## Use MaxConcurrentTaskCount to Control the Flow of Data Objects<a name="sms-batching-maxconcurrenttaskcount"></a>

[https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-MaxConcurrentTaskCount](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-MaxConcurrentTaskCount) defines the maximum number of data objects that can be labeled by human workers at the same time\. If you use the console, this parameter is set to 1,000\. If you use `CreateLabelingJob`, you can set this parameter to any integer between 1 and 1,000, inclusive\.

When you start a labeling job using an input manifest file, Ground Truth does the following: 

1. For each data object listed in your input manifest file, one or more tasks are created, depending on the value you specify for `NumberOfHumanWorkersPerDataObject`\. For example, if you set the number of workers per data object to 3, 3 tasks will be created for each dataset object\. To be marked as successfully labeled, at least one worker must label the object\. Alternatively, the tasks can expire or be declined\.

1. If you are using the Mechanical Turk workforce, Ground Truth first sends a batch of 10 dataset objects to your workers\. It uses this small batch to set up the labeling job and to make sure that the job is correctly configured\. 

1. Next, Ground Truth sends `MaxConcurrentTaskCount` number of dataset objects to workers\. For example, if you have 2,000 input data objects in your input manifest file and have set the number of workers per data object to 3 and set `MaxConcurrentTaskCount` to 900, the first 900 data objects in your input manifest are sent to workers, corresponding to 2,700 tasks \(900 x 3\)\. This is the first full\-sized set of objects sent to workers\.

1. What happens next depends on the type of labeling job you create\. This step assumes one or more dataset objects in your input manifest file, or sent using an Amazon SNS input data source \(in a streaming labeling job\) were not include in the set sent to workers in step 3\.
   + **Streaming labeling job**: As long as the total number of objects available to workers is equal to `MaxConcurrentTaskCount`, all remaining dataset objects on your input manifest file and that you send in real time using Amazon SNS are placed on an Amazon SQS queue\. When the total number of objects available to workers falls below `MaxConcurrentTaskCount` minus `NumberOfHumanWorkersPerDataObject`, a new data object from the queue is used to create `NumberOfHumanWorkersPerDataObject`\-tasks, which are sent to workers in real time\.
   + **Non\-streaming labeling job**: As workers finish labeling one set of objects, up to `MaxConcurrentTaskCount` times `NumberOfHumanWorkersPerDataObject` number of new tasks will be sent to workers\. This process is repeated until all data objects in the input manifest file are labeled\.

## Use Amazon SQS to Control the Flow of Data Objects to Streaming Labeling Jobs<a name="sms-batching-streaming-sqs"></a>

When you create a streaming labeling job, an Amazon SQS queue is automatically created in your account\. Data objects are only added to the Amazon SQS queue when the total number of objects sent to workers is above `MaxConcurrentTaskCount`\. Otherwise, objects are sent directly to workers\.

You can use this queue to manage the flow of data objects to your labeling job\. To learn more, see [Manage Labeling Requests with an Amazon SQS Queue ](sms-streaming-labeling-job.md#sms-streaming-how-it-works-sqs)\.