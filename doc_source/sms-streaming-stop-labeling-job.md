# Stop a Streaming Labeling Job<a name="sms-streaming-stop-labeling-job"></a>

You can manually stop your streaming labeling job using the operation [StopLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopLabelingJob.html)\. 

If your labeling job remains idle for over 10 days, it is automatically stopped by Ground Truth\. In this context, a labeling job is considered *idle* for 10 days if no new messages are sent to your Amazon SNS input topic in this time\. If this 10 day limit poses a problem for your business use case, please contact [AWS Support](http://aws.amazon.com/contact-us/)\. 

When a labeling job is stopped, its status is `STOPPING` while Ground Truth cleans up labeling job resources and unsubscribes your Amazon SNS topic from your Amazon SQS queue\. The Amazon SQS is *not* deleted by Ground Truth because this queue may contain unprocessed data objects\. You should manually delete the queue if you want to avoid incurring additional charges from Amazon SQS\. To learn more, see [Amazon SQS pricing ](https://aws.amazon.com/sqs/pricing/)\.