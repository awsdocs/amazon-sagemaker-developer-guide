# Monitor and Manage Your Human Loop<a name="a2i-monitor-humanloop-results"></a>


****  

|  | 
| --- |
|  Amazon Augmented AI is in preview release and is subject to change\. We do not recommend using this product in production environments\. | 

Once you've started a human review loop, you can check the results of and manage the loop using the [Amazon Augmented AI Runtime API](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/Welcome.html)\. Additionally, Amazon A2I integrates with Amazon EventBridge \(also known as Amazon CloudWatch Events\) to alert you when a human review loop changes status\. 

Use the procedures below to learn how to use the Amazon Augmented AI Runtime API to monitor and manage your human loops\. See [Use Amazon CloudWatch Events in Amazon Augmented AICloudWatch Events](augmented-ai-cloudwatch-events.md) to learn how Amazon A2I integrates with Amazon EventBridge\. 

**To check your output data:**

1. Check the results of your human loop by calling the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DescribeHumanLoop.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DescribeHumanLoop.html) operation\. The result of this API operation contains information about the reason for and outcome of the loop activation\.

1. Check the output data from your human loop in Amazon Simple Storage Service \(Amazon S3\)\. The path to the data uses the following pattern where `YYYY/MM/DD/hh/mm/ss` represents the human loop creation date with year \(`YYYY`\), month \(`MM`\) and day \(`DD`\) and the creation time with hour \(`hh`\), minute \(`mm`\) and second \(`ss`\)\. 

   ```
   s3://customer-output-bucket-specified-in-flow-definition/flow-definition-name/YYYY/MM/DD/hh/mm/ss/human-loop-name/output.json
   ```

You can integrate this structure with AWS Glue or Amazon Athena to partition and analyze your output data\. For more information, see [Managing Partitions for ETL Output in AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-partitions.html) and 

**To stop and delete your human loop:**

1. Once a human loop has been started, you can stop your human loop by calling the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StopHumanLoop.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StopHumanLoop.html) operation using the `HumanLoopName`\. If a human loop was successfully stopped, the server sends back an HTTP 200 response\. 

1. To delete a human loop for which the status equals `Failed`, `Completed`, or `Stopped`, use the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DeleteHumanLoop.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DeleteHumanLoop.html) operation\. 

**To list human loops:**

1. You can list all active human loops by calling the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_ListHumanLoops.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_ListHumanLoops.html) operation\. You can filter human loops by the creation date of the loop using the `CreationTimeAfter` and `CreateTimeBefore` parameters\. 

1. If successful, `ListHumanLoops` will return [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopSummary.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopSummary.html) and `NextToken` objects in the response element\. `HumanLoopSummaries` contains information about a single human loop\. For example, it will list a loop's status and if applicable, failure reason\. 

   Use the string returned in `NextToken` as an input in a subsequent call to `ListHumanLoops` to see the next page of human loops\. 