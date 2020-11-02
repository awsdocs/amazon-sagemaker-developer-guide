# Delete a Flow Definition<a name="a2i-delete-flow-definition"></a>

When you delete a flow definition, your flow definition status changes to `Deleting` and all associated human loops will automatically be stopped and deleted by Augmented AI if workers have not started tasks created by those human loops\. If human workers are already working on a task, that task will continue to be available until it is completed or expires\. As long as workers are still working on a task, your flow definition's status will be `Deleting`\. If these tasks are completed, the results are stored in the Amazon S3 bucket specified in your flow definition\. Deleting a flow definition won't remove any worker answers from your Amazon S3 bucket\. 

After all human loops have been deleted, the flow definition will be permanently deleted\. When a flow definition has been deleted, you can reuse its name to create a new flow definition\. 

You might want to delete a flow definition for any of the following reasons:
+ You have sent data to a set of human reviewers and you want to delete all non\-started human loops because you do not want those workers to work on those tasks any longer\.
+ The worker task template used to generate your worker UI does not render correctly or is not functioning as expected\. 

After you delete a flow definition, the following changes occur:
+ The flow definition no longer appears on the Human review workflows page in the Augmented AI area of the Amazon SageMaker console\. 
+ When you use the flow definition name as input to the API operations [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFlowDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFlowDefinition.html) or [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteFlowDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteFlowDefinition.html), Augmented AI returns a `ResourceNotFound` error\. 
+ When you use [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListFlowDefinitions.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListFlowDefinitions.html), deleted flow definitions won't be included in the results\. 
+ When use the flow definition ARN as input to the Augmented AI Runtime API operation `[ListHumanLoops](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_ListHumanLoops.html)`, Augmented AI returns a `ResourceNotFoundException`\.

## Delete a Flow Definition Using the Console or the SageMaker API<a name="a2i-delete-flow-definition-how-to"></a>

You can delete a flow definition on the Human review workflows page in the Augmented AI area of the SageMaker console or by using the SageMaker API\. 

Flow definitions can only be deleted if their status is `Active`\. 

**Delete a flow definition \(console\)**

1. Navigate to the SageMaker console at [https://console.aws.amazon.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, under the **Augmented AI** section, choose **Human review workflows**\.

1. Choose the hyperlinked name of the human review workflow that you want to delete\. 

1. On the **Summary** page of your human review workflow, choose **Delete** in the upper\-right corner\. 

1. In the dialog box asking you to confirm that you want to delete your human review workflow, choose **Delete**\. 

You're automatically redirected to the **Human review workflows** page\. While your human review workflow is being deleted, the status **Deleting** appears in the status column for that workflow\. After it's deleted, it won't appear in the list of workflows on this page\. 

**Delete a flow definition \(API\)**  
You can delete a flow definition using the SageMaker [DeleteFlowDefinition](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteFlowDefinition.html) API operation\. This API operation is supported through the [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/delete-flow-definition.html) and a [variety of language specific SDKs](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteFlowDefinition.html#API_DeleteFlowDefinition_SeeAlso)\. The following table shows example requests using SDK for Python \(Boto3\) and the AWS CLI to delete the flow definition, *`example-flow-definition`*\. 

------
#### [ AWS SDK for Python \(Boto3\) ]

The following request example uses the SDK for Python \(Boto3\) to delete the flow definition\. For more information, see [delete\_flow\_definition](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.delete_flow_definition) in the *AWS SDK for Python \(Boto\) API Reference*\.

```
response = client.delete_flow_definition(FlowDefinitionName='example-flow-definition')
```

------
#### [ AWS CLI ]

The following request example uses the AWS CLI to delete the flow definition\. For more information, see [delete\-flow\-definition](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/delete-flow-definition.html) in the *[AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)*\. 

```
$ delete-flow-definition --flow-definition-name 'example-flow-definition'
```

------

If the action is successful, Augmented AI sends back an HTTP 200 response with an empty HTTP body\.