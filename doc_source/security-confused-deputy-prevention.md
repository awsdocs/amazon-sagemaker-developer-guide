# Cross\-Service Confused Deputy Prevention<a name="security-confused-deputy-prevention"></a>

The [confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy) is a security issue where an entity that doesn't have permission to perform an action can coerce a more\-privileged entity to perform the action\. In AWS, cross\-service impersonation can result in the confused deputy problem\. Cross\-service impersonation can occur when one service \(the *calling service*\) calls another service \(the *called service*\)\. The calling service can be manipulated to use its permissions to act on another customer's resources in a way it should not otherwise have permission to access\. To prevent this, AWS provides tools that help you protect your data for all services with service principals that have been given access to resources in your account\. 

Read on for general guidance or navigate to an example for a specific SageMaker feature:

**Topics**
+ [Limit Permissions With Global Condition Keys](#security-confused-deputy-context-keys)
+ [SageMaker Edge Manager](#security-confused-deputy-edge-manager)
+ [SageMaker Images](#security-confused-deputy-images)
+ [SageMaker Inference](#security-confused-deputy-inference)
+ [SageMaker Batch Transform Jobs](#security-confused-deputy-batch)
+ [SageMaker Marketplace](#security-confused-deputy-marketplace)
+ [SageMaker Neo](#security-confused-deputy-neo)
+ [SageMaker Pipelines](#security-confused-deputy-pipelines)
+ [SageMaker Processing Jobs](#security-confused-deputy-processing-job)
+ [SageMaker Studio](#security-confused-deputy-studio)
+ [SageMaker Training Jobs](#security-confused-deputy-training-job)

## Limit Permissions With Global Condition Keys<a name="security-confused-deputy-context-keys"></a>

We recommend using the `[aws:SourceArn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn)` and `[aws:SourceAccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount)` global condition keys in resource policies to limit the permissions to the resource that Amazon SageMaker gives another service\. If you use both global condition keys and the `aws:SourceArn` value contains the account ID, the `aws:SourceAccount` value and the account in the `aws:SourceArn` value must use the same account ID when used in the same policy statement\. Use `aws:SourceArn` if you want only one resource to be associated with the cross\-service access\. Use `aws:SourceAccount` if you want to allow any resource in that account to be associated with the cross\-service use\.

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

## SageMaker Edge Manager<a name="security-confused-deputy-edge-manager"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for SageMaker Edge Manager created by account number *123456789012* in the *us\-west\-2* Region\.

```
{
  "Version": "2012-10-17",
  "Statement": {
      "Effect": "Allow",
      "Principal": { "Service": "sagemaker.amazonaws.com" },
      "Action": "sts:AssumeRole",
      "Condition": {
        "ArnLike": {
          "aws:SourceArn": "arn:aws:sagemaker:us-west-2:123456789012:*"
      }
    }
  }
}
```

You can replace the `aws:SourceArn` in this template with the full ARN of one specific packaging job to further limit permissions\.

## SageMaker Images<a name="security-confused-deputy-images"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for [SageMaker Images](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-byoi.html)\. Use this template with either `[Image](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Image.html)` or `[ImageVersion](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ImageVersion.html)`\. This example uses an `ImageVersion` record ARN with the account number *123456789012*\. Note that because the account number is part of the `aws:SourceArn` value, you do not need to specify an `aws:SourceAccount` value\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Principal": { "Service": "sagemaker.amazonaws.com" },
    "Action": "sts:AssumeRole",
    "Condition": {
      "ArnLike": {
        "aws:SourceArn": "arn:partition:sagemaker:us-west-2:123456789012:image-version"
      }
    }
  }
}
```

Do not replace the `aws:SourceArn` in this template with the full ARN of a specific image or image version\. The ARN must be in the format provided above and specify either `image` or `image-version`\. The `partition` placeholder should designate either an AWS commercial partition \(`aws`\) or an AWS in China partition \(`aws-cn`\), depending on where the image or image version is running\. Similarly, the `region` placeholder in the ARN can be any [valid Region](https://docs.aws.amazon.com/sagemaker/latest/dg/regions-quotas.html) where SageMaker images are available\.

## SageMaker Inference<a name="security-confused-deputy-inference"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for SageMaker [real\-time](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints), [serverless](https://docs.aws.amazon.com/sagemaker/latest/dg/serverless-endpoints), and [asynchronous](https://docs.aws.amazon.com/sagemaker/latest/dg/async-inference) inference\. Note that because the account number is part of the `aws:SourceArn` value, you do not need to specify an `aws:SourceAccount` value\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Principal": { "Service": "sagemaker.amazonaws.com" },
    "Action": "sts:AssumeRole",
    "Condition": {
      "ArnLike": {
        "aws:SourceArn": "arn:aws:sagemaker:us-west-2:123456789012:*"
      }
    }
  }
}
```

Do not replace the `aws:SourceArn` in this template with the full ARN of a specific model or endpoint\. The ARN must be in the format provided above\. The asterisk in the ARN template does not stand for wildcard and should not be changed\. 

## SageMaker Batch Transform Jobs<a name="security-confused-deputy-batch"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for SageMaker [batch transform jobs](https://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform.html) created by account number *123456789012* in the *us\-west\-2* Region\. Note that because the account number is in the ARN, you do not need to specify an `aws:SourceAccount` value\.

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
          "aws:SourceArn": "arn:aws:sagemaker:us-west-2:123456789012:transform-job/*"
        }
      }
    }
  ]
}
```

You can replace the `aws:SourceArn` in this template with the full ARN of one specific batch transform job to further limit permissions\. 

## SageMaker Marketplace<a name="security-confused-deputy-marketplace"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for SageMaker Marketplace resources created by account number *123456789012* in the *us\-west\-2* Region\. Note that because the account number is in the ARN, you do not need to specify an `aws:SourceAccount` value\. 

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
          "aws:SourceArn": "arn:aws:sagemaker:us-west-2:123456789012:*"
        }
      }
    }
  ]
}
```

Do not replace the `aws:SourceArn` in this template with the full ARN of a specific algorithm or model package\. The ARN must be in the format provided above\. The asterisk in the ARN template does stand for wildcard and covers all training jobs, models, and batch transform jobs from validation steps, as well as algorithm and model packages published to SageMaker Marketplace\. 

## SageMaker Neo<a name="security-confused-deputy-neo"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for SageMaker Neo compilation jobs created by account number *123456789012* in the *us\-west\-2* Region\. Note that because the account number is in the ARN, you do not need to specify an `aws:SourceAccount` value\.

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
          "aws:SourceArn": "arn:aws:sagemaker:us-west-2:123456789012:compilation-job/*"
        }
      }
    }
  ]
}
```

You can replace the `aws:SourceArn` in this template with the full ARN of one specific compilation job to further limit permissions\.

## SageMaker Pipelines<a name="security-confused-deputy-pipelines"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for [SageMaker Pipelines](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines-sdk.html) using pipeline execution records from one or more pipelines\. Note that because the account number is in the ARN, you do not need to specify an `aws:SourceAccount` value\.

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
          "aws:SourceArn": "arn:partition:sagemaker:region:123456789012:pipeline/mypipeline/*"
        }
      }
    }
  ]
}
```

Do not replace the `aws:SourceArn` in this template with the full ARN of a specific pipeline execution\. The ARN must be in the format provided above\. The `partition` placeholder should designate either an AWS commercial partition \(`aws`\) or an AWS in China partition \(`aws-cn`\), depending on where the pipeline is running\. Similarly, the `region` placeholder in the ARN can be any [valid Region](https://docs.aws.amazon.com/sagemaker/latest/dg/regions-quotas.html) where SageMaker Pipelines is available\.

The asterisk in the ARN template does stand for wildcard and covers all pipeline executions of a pipeline named `mypipeline`\. If you want to allow the `AssumeRole` permissions for all pipelines in account `123456789012` rather than one specific pipeline, then the `aws:SourceArn` would be `arn:aws:sagemaker:*:123456789012:pipeline/*`\.

## SageMaker Processing Jobs<a name="security-confused-deputy-processing-job"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for SageMaker processing jobs created by account number *123456789012* in the *us\-west\-2* Region\. Note that because the account number is in the ARN, you do not need to specify an `aws:SourceAccount` value\.

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
          "aws:SourceArn": "arn:aws:sagemaker:us-west-2:123456789012:processing-job/*"
        }
      }
    }
  ]
}
```

You can replace the `aws:SourceArn` in this template with the full ARN of one specific processing job to further limit permissions\.

## SageMaker Studio<a name="security-confused-deputy-studio"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for SageMaker Studio created by account number *123456789012* in the *us\-west\-2* Region\. Note that because the account number is part of the `aws:SourceArn` value, you do not need to specify an `aws:SourceAccount` value\. 

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
          "aws:SourceArn": "arn:aws:sagemaker:us-west-2:123456789012:*"
        }
      }
    }
  ]
}
```

Do not replace the `aws:SourceArn` in this template with the full ARN of a specific Studio application, user profile, or domain\. The ARN must be in the format provided in the previous example\. The asterisk in the ARN template does not stand for wildcard and should not be changed\.

## SageMaker Training Jobs<a name="security-confused-deputy-training-job"></a>

The following example shows how you can use the `aws:SourceArn` global condition key to prevent the cross\-service confused deputy problem for SageMaker training jobs created by account number *123456789012* in the *us\-west\-2* Region\. Note that because the account number is in the ARN, you do not need to specify an `aws:SourceAccount` value\.

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