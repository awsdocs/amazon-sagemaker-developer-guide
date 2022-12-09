# Multiple Domains Overview<a name="domain-multiple"></a>

 Amazon SageMaker supports the creation of multiple Amazon SageMaker Domains in a single AWS Region for each account\. Additional Domains in a Region have the same features and capabilities as the first Domain in a Region\. Each Domain can have distinct Domain settings\. User profiles cannot be added to multiple Domains in a single Region within the same account\. For more information about Domain limits, see [Amazon SageMaker endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html)\. 

**Topics**
+ [Scoping each Domain](#domain-multiple-scoping)
+ [Backfilling Domain tags](#domain-multiple-backfill)

## Scoping each Domain<a name="domain-multiple-scoping"></a>

 By default, any SageMaker resources that support tagging created in a Domain after 11/30/2022 are automatically tagged with a Domain ARN tag that is based on the Domain ID of that Domain\. You can use these tags to enable Domain\-level SageMaker resource isolation\. With resource isolation, SageMaker resources, such as models, experiments, training jobs, and pipelines created in one Domain, cannot be accessed from other Domains\.   

 You can also use these tags for cost allocation using AWS Billing and Cost Management\. For more information, see [Using AWS cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)\. 

 To enable resource isolation, you must modify the IAM execution role of your Domain, as follows\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
  {
    "Sid": "All resource creation requires the following tags",
    "Effect": "Allow",
    "Action": [
      "SageMaker:Create*",
      "SageMaker:Update*"
    ],
    "Resource": "*",
    "Condition": {
      "ForAllValues:StringEquals": {
        "aws:TagKeys": [
          "sagemaker:domain-arn"
        ]
      }
    }
  },
  {
    "Sid": "Resource access matches tag value of sagemaker:domain-arn",
    "Effect": "Allow",
    "Action": [
        "SageMaker:Update*",
        "SageMaker:Delete*",
        "SageMaker:Describe*"
    ],
    "Resource": "*",
    "Condition": {
        "StringEquals": {
          "aws:ResourceTag/sagemaker:domain-arn": "domain-arn"
        }
      }
    }
  ]
}
```

## Backfilling Domain tags<a name="domain-multiple-backfill"></a>

 If you have created resources in a Domain before 11/30/2022, those resources are not automatically tagged with the Domain Amazon Resource Name \(ARN\) tag\. 

To accurately attribute resources to their respective Domain, you must add the Domain tag to existing resources using the AWS CLI, as follows\.

1. Map all existing SageMaker resources and their respective ARNs to the Domains that exist in your account\.

1. Run the following command from your local machine to tag the resource with the ARN of the resource's respective Domain\. This must be repeated for every SageMaker resource in your account\.

   ```
   aws resourcegroupstaggingapi tag-resources \
       --resource-arn-list arn:aws:sagemaker:region:account-id:space/domain-id/space-name \
       --tags sagemaker:domain-arn=arn:aws:sagemaker:region:account-id:domain/domain-id
   ```