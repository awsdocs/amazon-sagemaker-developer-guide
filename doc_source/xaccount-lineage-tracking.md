# Cross\-Account Lineage Tracking<a name="xaccount-lineage-tracking"></a>

Amazon SageMaker supports tracking lineage entities from a different AWS account\. Other AWS accounts can share their lineage entities with you and you can access these lineage entities through direct API calls or SageMaker lineage queries\.

SageMaker uses [AWS Resource Access Manager](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html) to help you securely share your lineage resources\. You can share your resources through the [AWS RAM console](https://console.aws.amazon.com/ram/home)\.



## Set Up Cross\-Account Lineage Tracking<a name="setup-xaccount-lineage-tracking"></a>

You can group and share your [Lineage Tracking Entities](lineage-tracking-entities.md) through a lineage group in Amazon SageMaker\. SageMaker supports only one default lineage group per account\. SageMaker creates the default lineage group whenever a lineage entity is created in your account\. Every lineage entity owned by your account is assigned to this default lineage group\. To share lineage entities with another account, you share this default lineage group with that account\.

**Note**  
You can share all lineage tracking entities in a lineage group or none\.

Create a resource share for your lineage entities using AWS Resource Access Manager console\. For more information, see [Sharing your AWS resources](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html) in the *AWS Resource Access Manager User Guide*\.

**Note**  
After the resource share is created, it can take a few minutes for the resource and principal associations to complete\. Once the association is set, the shared account receives an invitation to join the resource share\. The shared account must accept the invite to gain access to shared resources\. For more information on accepting a resource share invite in AWS RAM, see [Using shared AWS resources ](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-shared.html) in the *AWS Resource Access Manager User Guide*\.

### Your cross\-account lineage tracking resource policy<a name="setup-xaccount-lineage-tracking"></a>

Amazon SageMaker supports only one type of resource policy\. The SageMaker resource policy must allow all of the following operations:

```
"sagemaker:DescribeAction"
"sagemaker:DescribeArtifact"
"sagemaker:DescribeContext"
"sagemaker:DescribeTrialComponent"
"sagemaker:AddAssociation"
"sagemaker:DeleteAssociation"
"sagemaker:QueryLineage"
```

**Example The following is a SageMaker resource policy created using AWS Resource Access Manager for creating a resource share for an accounts lineage group\.**  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FullLineageAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "123456789012" #account-id
      },
      "Action": [
        "sagemaker:DescribeAction",
        "sagemaker:DescribeArtifact",
        "sagemaker:DescribeContext",
        "sagemaker:DescribeTrialComponent",
        "sagemaker:AddAssociation",
        "sagemaker:DeleteAssociation",
        "sagemaker:QueryLineage"
      ],
      "Resource": "arn:aws:sagemaker:us-west-2:111111111111:lineage-group/sagemaker-default-lineage-group" #Sample lineage group resource 
    }
  ]
}
```

## Tracking Cross\-Account Lineage Entities<a name="tracking-lineage-xaccount"></a>

With cross\-account lineage tracking you can associate lineage entities in different accounts using the same `AddAssociation` API action\. When you associate two lineage entities, SageMaker validates if you have permissions to perform the `AddAssociation` API action on both lineage entities\. SageMaker then establishes the association\. If you don’t have the permissions, SageMaker *does not* create the association\. Once the cross\-account association is established, you can access either lineage entity from the other through the `QueryLineage` API action\. For more information, see [Querying Lineage Entities](querying-lineage-entities.md)\.

In addition to SageMaker automatically creating lineage entities, if you have cross\-account access, SageMaker connects artifacts that reference the same object or data\. If the data from one account is used in lineage tracking by different accounts, SageMaker creates an artifact in each account to track that data\. With cross\-account lineage, whenever SageMaker creates new artifacts, SageMaker checks if there are other artifacts created for the same data that are also shared with you\. SageMaker then establishes associations between the newly created artifact and each of the artifacts shared with you with the `AssociationType` set to `SameAs`\. You can then use the `[QueryLineage](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_QueryLineage.html)` API action to traverse the lineage entities in your own account to lineage entities shared with you but owned by a different AWS account\. For more information, see [Querying Lineage Entities](querying-lineage-entities.md)

**Topics**
+ [Accessing lineage resources from a different accounts](#tracking-lineage-xaccount-accessing-resources)
+ [Authorization for querying cross\-account lineage entities](#tracking-lineage-xaccount-authorization)

### Accessing lineage resources from a different accounts<a name="tracking-lineage-xaccount-accessing-resources"></a>

Once the cross\-account access for sharing lineage has been set up, you can call the following SageMaker API actions directly with the ARN to describe the shared lineage entities from another account:
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAction.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAction.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeArtifact.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeArtifact.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeContext.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeContext.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrialComponent.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrialComponent.html)

You can also manage [Associations ](https://docs.aws.amazon.com/sagemaker/latest/dg/lineage-tracking-entities.html)for lineage entities owned by different accounts that are shared with you, using the following SageMaker API actions: 
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AddAssociation.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AddAssociation.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteAssociation.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteAssociation.html)

For a notebook that demonstrates how to use SageMaker Lineage APIs to query lineage across accounts\., see [sagemaker\-lineage\-cross\-account\-with\-ram\.ipynb](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-lineage/sagemaker-lineage-cross-account-with-ram.ipynb)\.

### Authorization for querying cross\-account lineage entities<a name="tracking-lineage-xaccount-authorization"></a>

Amazon SageMaker must validate that you have permissions to perform the `QueryLineage` API action on the `StartArns`\. This is enforced through the resource policy attached to the `LineageGroup`\.  The result from this action includes all the lineage entities to which you have access, whether they are owned by your account or shared by another account\. For more information, see [Querying Lineage Entities](querying-lineage-entities.md)\.