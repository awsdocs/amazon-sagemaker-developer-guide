# Delete a Human Loop<a name="a2i-delete-human-loop"></a>

When you delete a human loop, the status changes to `Deleting`\. When the human loop is deleted, the associated human review task is no longer available to workers\. You might want to delete a human loop in one of the following circumstances:
+ The worker task template used to generate your worker user interface does not render correctly or is not functioning as expected\. 
+ A single data object was accidentally sent to workers multiple times\. 
+ You no longer need a data object reviewed by a human\. 

If the status of a human loop is `InProgress`, you must stop the human loop before deleting it\. When you stop a human loop, the status changes to `Stopping` while it is being stopped\. When the status changes to `Stopped`, you can delete the human loop\. 

If human workers are already working on a task when you stop the associated human loop, that task continues to be available until it is completed or expires\. As long as workers are still working on a task, your human loop's status is `Stopping`\. If these tasks are completed, the results are stored in the Amazon S3 bucket URI specified in your human review workflow\. If the worker leaves the task without submitting work, it is stopped and the worker can't return to the task\. If no worker has started working on the task, it is stopped immediately\. 

If you delete the AWS account used to create the human loop, it is stopped and deleted automatically\. 

## Human Loop Data Retention and Deletion<a name="a2i-delete-human-loop-data-retention"></a>

When a human worker completes a human review task, the results are stored in the Amazon S3 output bucket you specified in the human review workflow used to create the human loop\. Deleting or stopping a human loop does not remove any worker answers from your S3 bucket\. 

Additionally, Amazon A2I temporarily stores human loop input and output data internally for the following reasons:
+ If you configure your human loops so that a single data object is sent to multiple workers for review, Amazon A2I does not write output data to your S3 bucket until all workers have completed the review task\. Amazon A2I stores partial answers—answers from individual workers—internally so that it can write full results to your S3 bucket\. 
+ If you report a low\-quality human review result, Amazon A2I can investigate and respond to your issue\. 
+ If you lose access to or delete the output S3 bucket specified in the human review workflow used to create a human loop, and the task has already been sent to one or more workers, Amazon A2I needs a place to temporarily store human review results\. 

Amazon A2I deletes this data internally 30 days after a human loop's status changes to one of the following: `Deleted`, `Stopped`, or `Completed`\. In other words, data is deleted 30 days after the human loop has been completed, stopped, or deleted\. Additionally, this data is deleted after 30 days if you close the AWS account used to create associated human loops\.

## Stop and Delete a Flow Definition Using the Console or the Amazon A2I API<a name="a2i-delete-human-loop-how-to"></a>

You can stop and delete a human loop in the Augmented AI console or by using the SageMaker API\. When the human loop has been deleted, the status changes to `Deleted`\.

**Delete a human loop \(console\)**

1. Navigate to the Augmented AI console at [https://console\.aws\.amazon\.com/a2i/](https://console.aws.amazon.com/a2i/)\.

1. In the navigation pane, under the **Augmented AI** section, choose **Human review workflows**\.

1. Choose the hyperlinked name of the human review workflow you used to create the human loop you want to delete\. 

1. In the **Human loops** section at the bottom of the page, select the human loop you want to stop and delete\.

1. If the human loop status is `Completed`, `Stopped`, or `Failed`, select **Delete**\.

   If the human loop **Status** is `InProgress`, select **Stop**\. When the status changes to **Stopped**, select **Delete**\.

**Delete a human loop \(API\)**

1. Check the status of your human loop using the Augmented AI Runtime API operation `[DescribeHumanLoop](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DescribeHumanLoop.html)`\. See examples using this operation in the following table\. 

------
#### [ AWS SDK for Python \(Boto3\) ]

   The following example uses the SDK for Python \(Boto3\) to describe the human loop named *example\-human\-loop*\. For more information, see [describe\_human\_loop](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.describe_human_loop) in the *AWS SDK for Python \(Boto\) API Reference*\.

   ```
   import boto3
   
   a2i_runtime_client = boto3.client('sagemaker-a2i-runtime')
   response = a2i_runtime_client.describe_human_loop(HumanLoopName='example-human-loop')
   human_loop_status = response['HumanLoopStatus']
   print(f'example-human-loop status is: {human_loop_status}')
   ```

------
#### [ AWS CLI ]

   The following example uses the AWS CLI to describe the human loop named *example\-human\-loop*\. For more information, see [describe\-human\-loop](https://docs.aws.amazon.com/cli/latest/reference/sagemaker-a2i-runtime/describe-human-loop.html) in the *[AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)*\. 

   ```
   $ aws sagemaker-a2i-runtime describe-human-loop --human-loop-name 'example-human-loop'
   ```

------

1. If the flow definition status is `Completed`, `Stopped`, or `Failed`, delete the flow definition using the Augmented AI Runtime API operation `[DeleteHumanLoop](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DeleteHumanLoop.html)`\.

------
#### [ AWS SDK for Python \(Boto3\) ]

   The following example uses the SDK for Python \(Boto3\) to delete the human loop named *example\-human\-loop*\. For more information, see [delete\_human\_loop](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.delete_human_loop) in the *AWS SDK for Python \(Boto\) API Reference*\.

   ```
   import boto3
   
   a2i_runtime_client = boto3.client('sagemaker-a2i-runtime')
   response = a2i_runtime_client.delete_human_loop(HumanLoopName='example-human-loop')
   ```

------
#### [ AWS CLI ]

   The following example uses the AWS CLI to delete the human loop named *example\-human\-loop*\. For more information, see [delete\-human\-loop](https://docs.aws.amazon.com/cli/latest/reference/sagemaker-a2i-runtime/delete-human-loop.html) in the *[AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)*\. 

   ```
   $ aws sagemaker-a2i-runtime delete-human-loop --human-loop-name 'example-human-loop'
   ```

------

   If the human loop status is `InProgress`, stop the human loop using `[StopHumanLoop](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StopHumanLoop.html)` and then use `DeleteHumanLoop` to delete it\. 

------
#### [ AWS SDK for Python \(Boto3\) ]

   The following example uses the SDK for Python \(Boto3\) to describe the human loop named *example\-human\-loop*\. For more information, see [stop\_human\_loop](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.stop_human_loop) in the *AWS SDK for Python \(Boto\) API Reference*\.

   ```
   import boto3
   
   a2i_runtime_client = boto3.client('sagemaker-a2i-runtime')
   response = a2i_runtime_client.stop_human_loop(HumanLoopName='example-human-loop')
   ```

------
#### [ AWS CLI ]

   The following example uses the AWS CLI to describe the human loop named *example\-human\-loop*\. For more information, see [stop\-human\-loop](https://docs.aws.amazon.com/cli/latest/reference/sagemaker-a2i-runtime/stop-human-loop.html) in the *[AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)*\. 

   ```
   $ aws sagemaker-a2i-runtime stop-human-loop --human-loop-name 'example-human-loop'
   ```

------