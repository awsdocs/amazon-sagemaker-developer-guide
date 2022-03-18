# Cross\-Service Confused Deputy Prevention<a name="security-confused-deputy-prevention"></a>

The [confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy) is a security issue where an entity that doesn't have permission to perform an action can coerce a more\-privileged entity to perform the action\. In AWS, cross\-service impersonation can result in the confused deputy problem\. Cross\-service impersonation can occur when one service \(the *calling service*\) calls another service \(the *called service*\)\. The calling service can be manipulated to use its permissions to act on another customer's resources in a way it should not otherwise have permission to access\. To prevent this, AWS provides tools that help you protect your data for all services with service principals that have been given access to resources in your account\. 

## Limit Permissions With Global Condition Keys<a name="security-confused-deputy-context-keys"></a>

We recommend using the `[aws:SourceArn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn)` and `[aws:SourceAccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount)` global condition keys in resource policies to limit the permissions that Amazon SageMaker gives another service to the resource\. If you use both global condition keys and the `aws:SourceArn` value contains the account ID, the `aws:SourceAccount` value and the account in the `aws:SourceArn` value must use the same account ID when used in the same policy statement\. Use `aws:SourceArn` if you want only one resource to be associated with the cross\-service access\. Use `aws:SourceAccount` if you want to allow any resource in that account to be associated with the cross\-service use\.

The most effective way to protect against the confused deputy problem is to use the `aws:SourceArn` global condition key with the full ARN of the resource\. If you don't know the full ARN of the resource or if you are specifying multiple resources, use the `aws:SourceArn` global condition key with wildcards \(`*`\) for the unknown portions of the ARN\. For example, `arn:aws:sagemaker:*:123456789012:*`\.

The following example shows how you can use the `aws:SourceArn` and `aws:SourceAccount` global condition keys in SageMaker to prevent the confused deputy problem\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "ConfusedDeputyPreventionExamplePolicy",
    "Effect": "Allow",
    "Principal": {
      "Service": "sagemaker.amazonaws.com"
    },
    # Specify an action and resource policy for another service
    "Action": "service:ActionName",
    "Resource": [
      "arn:aws:service:::ResourceName/*"
    ],
    "Condition": {
      "ArnLike": {
        "aws:SourceArn": "arn:partition:sagemaker:region:123456789012:*"
      },
      "StringEquals": {
        "aws:SourceAccount": "123456789012"
      }
    }
  }
}
```

## Cross\-Service Confused Deputy Prevention for SageMaker Training Jobs<a name="security-confused-deputy-training-job"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for SageMaker training jobs created by account number *123456789012* in region *us\-west\-2*\. Note that because the account number is in the ARN, you do not need to specify an `aws:SourceAccount` value\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "sagemaker.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "ArnLike": {
          "aws:SourceArn": "arn:aws:sagemaker:us-west-2:123456789012:training-job/*"
        }
      }
    }
  ]
}
```

You can replace the `aws:SourceArn` in this template with the full ARN of one specific training job to further limit permissions\.

**Next Up**  
For more information on managing execution roles, see [SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles)\.