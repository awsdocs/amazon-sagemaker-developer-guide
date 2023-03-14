# Troubleshooting Amazon SageMaker Identity and Access<a name="security_iam_troubleshoot"></a>

Use the following information to help you diagnose and fix common issues that you might encounter when working with SageMaker and IAM\.

**Topics**
+ [I Am Not Authorized to Perform an Action in SageMaker](#security_iam_troubleshoot-no-permissions)
+ [I Am Not Authorized to Perform iam:PassRole](#security_iam_troubleshoot-passrole)
+ [I Want to Allow People Outside of My AWS Account to Access My SageMaker Resources](#security_iam_troubleshoot-cross-account-access)

## I Am Not Authorized to Perform an Action in SageMaker<a name="security_iam_troubleshoot-no-permissions"></a>

If the AWS Management Console tells you that you're not authorized to perform an action, then you must contact your administrator for assistance\. Your administrator is the person that provided you with your sign\-in credentials\.

The following example error occurs when the `mateojackson` IAM user tries to use the console to view details about a training job but does not have `sagemaker:sagemaker:DescribeTrainingJob` permissions\.

```
User: arn:aws:iam::123456789012:user/mateojackson is not
            authorized to perform: sagemaker:DescribeTrainingJob on resource: my-example-widget
```

In this case, Mateo asks his administrator to update his policies to allow him to access the `TrainingJob` resource using the `sagemaker:DescribeTrainingJob` action\.

## I Am Not Authorized to Perform iam:PassRole<a name="security_iam_troubleshoot-passrole"></a>

If you receive an error that you're not authorized to perform the `iam:PassRole` action, your policies must be updated to allow you to pass a role to SageMaker\.

Some AWS services allow you to pass an existing role to that service instead of creating a new service role or service\-linked role\. To do this, you must have permissions to pass the role to the service\.

The following example error occurs when an IAM user named `marymajor` tries to use the console to perform an action in SageMaker\. However, the action requires the service to have permissions that are granted by a service role\. Mary does not have permissions to pass the role to the service\.

```
User: arn:aws:iam::123456789012:user/marymajor is not authorized to perform: iam:PassRole
```

In this case, Mary's policies must be updated to allow her to perform the `iam:PassRole` action\.

If you need help, contact your AWS administrator\. Your administrator is the person who provided you with your sign\-in credentials\.

## I Want to Allow People Outside of My AWS Account to Access My SageMaker Resources<a name="security_iam_troubleshoot-cross-account-access"></a>

You can create a role that users in other accounts or people outside of your organization can use to access your resources\. You can specify who is trusted to assume the role\. For services that support resource\-based policies or access control lists \(ACLs\), you can use those policies to grant people access to your resources\.

To learn more, consult the following:
+ To learn whether SageMaker supports these features, see [How Amazon SageMaker Works with IAM](security_iam_service-with-iam.md)\.
+ To learn how to provide access to your resources across AWS accounts that you own, see [Providing access to an IAM user in another AWS account that you own](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_aws-accounts.html) in the *IAM User Guide*\.
+ To learn how to provide access to your resources to third\-party AWS accounts, see [Providing access to AWS accounts owned by third parties](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_third-party.html) in the *IAM User Guide*\.
+ To learn how to provide access through identity federation, see [Providing access to externally authenticated users \(identity federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_federated-users.html) in the *IAM User Guide*\.
+ To learn the difference between using roles and resource\-based policies for cross\-account access, see [How IAM roles differ from resource\-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html) in the *IAM User Guide*\.