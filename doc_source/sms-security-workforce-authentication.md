# Workforce Authentication and Restrictions<a name="sms-security-workforce-authentication"></a>

Ground Truth enables you to use your own private workforce to work on labeling jobs\. A *private workforce* is an abstract concept and it refers to a set of people who work for you\. Each labeling job is created using a work team, composed of workers in your workforce\. Ground Truth supports private workforce creation using Amazon Cognito\. 

A Ground Truth workforce maps to a Cognito user pool\. A Ground Truth work team maps to a Coginto user group\. Coginito manages the worker authentication\. Cognito supports Open ID connection \(OIDC\) and customers can set up Cognito federation with their own identity provider \(IdP\)\. 

Ground Truth only allows one workforce per account per AWS Region\. Each workforce has a dedicated Ground Truth work portal login URL\. 

You can also restrict workers to a Classless Inter\-Domain Routing \(CIDR\) block/IP address range\. This means annotators must be on a specific network to access the annotation site\. You can add up to ten CIDR blocks for one workforce\. To learn more, see [Manage Private Workforce Using the Amazon SageMaker API](sms-workforce-management-private-api.md)\.

To learn how you can create a private workforce, see [Create a Private Workforce \(Amazon Cognito\)](sms-workforce-create-private.md)\.

## Restrict Access to Workforce Types<a name="sms-security-permission-condition-keys"></a>

Amazon SageMaker Ground Truth work teams fall into one of three [workforce types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management.html): public \(with Amazon Mechanical Turk\), private, and vendor\. To restrict IAM user access to a specific work team using one of these types or the work team ARN, use the `sagemaker:WorkteamType` and/or the `sagemaker:WorkteamArn` condition keys\. For the `sagemaker:WorkteamType` condition key, use [string condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_String)\. For the `sagemaker:WorkteamArn` condition key, use [Amazon Resource Name \(ARN\) condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_ARN)\. If the user attempts to create a labeling job with a restricted work team, SageMaker returns an access denied error\. 

The policies below demonstrate different ways to use the `sagemaker:WorkteamType` and `sagemaker:WorkteamArn` condition keys with appropriate condition operators and valid condition values\. 

The following example uses the `sagemaker:WorkteamType` condition key with the `StringEquals` condition operator to restrict access to a public work team\. It accepts condition values in the following format: `workforcetype-crowd`, where *workforcetype* can equal `public`, `private`, or `vendor`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "RestrictWorkteamType",
            "Effect": "Deny",
            "Action": "sagemaker:CreateLabelingJob",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "sagemaker:WorkteamType": "public-crowd"
                }
            }
        }
    ]
}
```

The following policies show how to restrict access to a public work team using the `sagemaker:WorkteamArn` condition key\. The first shows how to use it with a valid IAM regex\-variant of the work team ARN and the `ArnLike` condition operator\. The second shows how to use it with the `ArnEquals` condition operator and the work team ARN\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "RestrictWorkteamType",
            "Effect": "Deny",
            "Action": "sagemaker:CreateLabelingJob",
            "Resource": "*",
            "Condition": {
                "ArnLike": {
                    "sagemaker:WorkteamArn": "arn:aws:sagemaker:*:*:workteam/public-crowd/*"
                }
            }
        }
    ]
}
```

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "RestrictWorkteamType",
            "Effect": "Deny",
            "Action": "sagemaker:CreateLabelingJob",
            "Resource": "*",
            "Condition": {
                "ArnEquals": {
                    "sagemaker:WorkteamArn": "arn:aws:sagemaker:us-west-2:394669845002:workteam/public-crowd/default"
                }
            }
        }
    ]
}
```