# Monitor and Manage Your Human Loop<a name="a2i-monitor-humanloop-results"></a>

Once you've started a human review loop, you can check the results of and manage the loop using the [Amazon Augmented AI Runtime API](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/Welcome.html)\. Additionally, Amazon A2I integrates with Amazon EventBridge \(also known as Amazon CloudWatch Events\) to alert you when a human review loop changes status\. 

Use the procedures below to learn how to use the Amazon A2I Runtime API to monitor and manage your human loops\. See [Use Amazon CloudWatch Events in Amazon Augmented AICloudWatch Events](a2i-cloudwatch-events.md) to learn how Amazon A2I integrates with Amazon EventBridge\. 

**To check your output data:**

1. Check the results of your human loop by calling the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DescribeHumanLoop.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DescribeHumanLoop.html) operation\. The result of this API operation contains information about the reason for and outcome of the loop activation\.

1. Check the output data from your human loop in Amazon Simple Storage Service \(Amazon S3\)\. The path to the data uses the following pattern where `YYYY/MM/DD/hh/mm/ss` represents the human loop creation date with year \(`YYYY`\), month \(`MM`\) and day \(`DD`\) and the creation time with hour \(`hh`\), minute \(`mm`\) and second \(`ss`\)\. 

   ```
   s3://customer-output-bucket-specified-in-flow-definition/flow-definition-name/YYYY/MM/DD/hh/mm/ss/human-loop-name/output.json
   ```

You can integrate this structure with AWS Glue or Amazon Athena to partition and analyze your output data\. For more information, see [Managing Partitions for ETL Output in AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-partitions.html)\. 

**To stop and delete your human loop:**

1. Once a human loop has been started, you can stop your human loop by calling the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StopHumanLoop.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StopHumanLoop.html) operation using the `HumanLoopName`\. If a human loop was successfully stopped, the server sends back an HTTP 200 response\. 

1. To delete a human loop for which the status equals `Failed`, `Completed`, or `Stopped`, use the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DeleteHumanLoop.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DeleteHumanLoop.html) operation\. 

**To list human loops:**

1. You can list all active human loops by calling the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_ListHumanLoops.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_ListHumanLoops.html) operation\. You can filter human loops by the creation date of the loop using the `CreationTimeAfter` and `CreateTimeBefore` parameters\. 

1. If successful, `ListHumanLoops` will return [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopSummary.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopSummary.html) and `NextToken` objects in the response element\. `HumanLoopSummaries` contains information about a single human loop\. For example, it will list a loop's status and if applicable, failure reason\. 

   Use the string returned in `NextToken` as an input in a subsequent call to `ListHumanLoops` to see the next page of human loops\. 

## Track Private Worker Activity<a name="a2i-worker-id-private"></a>

If you used a private workforce for your human review tasks, Amazon A2I provides information that you can use to track individual workers in task output data\. To identify the worker that worked on the human review task, use the following from the output data in Amazon S3:
+ The `workerId` is unique to each worker\. 
+ In `workerMetadata`, you will see the following\.
  + `identityProviderType` – The service used to manage the private workforce\. 
  + `issuer` – The Cognito user pool or OIDC Identity Provider \(IdP\) issuer associated with the work team assigned to this human review task\.
  + `sub` – A unique identifier that refers to the worker\. If you created a workforce using Amazon Cognito, you can retrieve details about this worker \(such as the name or username\) using this ID using Amazon Cognito\. To learn how, see [Managing and Searching for User Accounts](https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-manage-user-accounts.html#manage-user-accounts-searching-user-attributes) in [Amazon Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/)\.

The following is an example of the output you may see if you used Amazon Cognito to create a private workforce:

```
"workerId": "a12b3cdefg4h5i67",
            "workerMetadata": {
                "identityData": {
                    "identityProviderType": "Cognito",
                    "issuer": "https://cognito-idp.aws-region.amazonaws.com/aws-region_123456789",
                    "sub": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
```

 The following is an example of the output you may see if you used your own OIDC IdP to create a private workforce:

```
"workerId": "a12b3cdefg4h5i67",
            "workerMetadata": {
                "identityData": {
                    "identityProviderType": "Oidc",
                    "issuer": "https://example-oidc-ipd.com/adfs",
                    "sub": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
```

To learn more about using private workforces, see [Use a Private Workforce](sms-workforce-private.md)\.