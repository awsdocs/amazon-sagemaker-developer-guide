# Monitoring user resource access from Amazon SageMaker Studio<a name="monitor-user-access"></a>

With Amazon SageMaker Studio, you can monitor user resource access\. To view resource access activity, you can configure AWS CloudTrail to monitor and record user activities by following the steps in [Log Amazon SageMaker API Calls with AWS CloudTrail](https://docs.aws.amazon.com/sagemaker/latest/dg/logging-using-cloudtrail.html)\. 

However, the AWS CloudTrail logs for resource access only list the Studio execution IAM role as the identifier\. This level of logging is enough to audit user activity when each user profile has a distinct execution role\. However, when a single execution IAM role is shared between several user profiles, you can't get information about the specific user that accessed the AWS resources\.  

You can get information about which specific user performed an action in an AWS CloudTrail log when using a shared execution role, using the `sourceIdentity` configuration to propagate the Studio user profile name\. For more information about source identity, see [Monitor and control actions taken with assumed roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_monitor.html)\. 

## Prerequisites<a name="monitor-user-access-prereq"></a>
+ Install and configure the AWS Command Line Interface following the steps in [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.
+ Ensure that Studio users in your domain don’t have a policy that allows them to update or modify the Studio domain\.  
+ All execution roles must have the following trust policy permissions: 
  + Any role that the Studio domain's execution role assumes must have the `sts:SetSourceIdentity` permission in the trust policy as follows\.

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                    "Service": "sagemaker.amazonaws.com"
                },
                "Action": [
                    "sts:AssumeRole",
                    "sts:SetSourceIdentity"
                ]
            }
        ]
    }
    ```
  + When you assume a role with another role, called role chaining, do the following:
    + Permissions for `sts:SetSourceIdentity` are required in both the permissions policy of the principal that is assuming the role, and in the role trust policy of the target role\. Otherwise, the assume role operation will fail\.
    +  This role chaining can happen in Studio or any other downstream service, such as Amazon EMR\. For more information about role chaining, see [Roles terms and concepts](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html)\. 

## Enable `sourceIdentity`<a name="monitor-user-access-enable"></a>

The ability to propagate the user profile name as the `sourceIdentity` in Studio is turned off by default\.

To enable the ability to propagate the user profile name as the `sourceIdentity`, use the AWS CLI during domain creation and domain update\. This feature is enabled at the domain level and not at the user profile level\.

 After you enable this configuration, administrators can view the user profile in the AWS CloudTrail log for the service accessed\. The user profile is given as the `sourceIdentity` value in the `userIdentity` section\. For more information about using AWS CloudTrail logs with SageMaker, see [Log Amazon SageMaker API Calls with AWS CloudTrail](https://docs.aws.amazon.com/sagemaker/latest/dg/logging-using-cloudtrail.html)\. 

You can use the following code to enable the propagation of the user profile name as the `sourceIdentity` during domain creation using the `create-domain` API\. 

```
create-domain
--domain-name <value>
--auth-mode <value>
--default-user-settings <value>
--subnet-ids <value>
--vpc-id <value>
[--tags <value>]
[--app-network-access-type <value>]
[--home-efs-file-system-kms-key-id <value>]
[--kms-key-id <value>]
[--app-security-group-management <value>]
[--domain-settings "ExecutionRoleIdentityConfig=USER_PROFILE_NAME"]
[--cli-input-json <value>]
[--generate-cli-skeleton <value>]
```

You can enable the propagation of the user profile name as the `sourceIdentity` during domain update using the `update-domain` API\.

To update this configuration, all apps in the domain must be in the `Stopped` or `Deleted` state\. For more information about how to stop and shut down apps, see [Shut down and Update Studio Apps](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-update-apps.html)\.

Use the following code to enable the propagation of the user profile name as the `sourceIdentity`\.

```
update-domain
--domain-id <value>
[--default-user-settings <value>]
[--domain-settings-for-update "ExecutionRoleIdentityConfig=USER_PROFILE_NAME"]
[--cli-input-json <value>]
[--generate-cli-skeleton <value>]
```

## Turn off `sourceIdentity`<a name="monitor-user-access-disable"></a>

 You can also turn off the propagation of the user profile name as the `sourceIdentity` using the AWS CLI\. This occurs during domain update by passing the `DISABLED` value for the `--role-session-context-config` parameter as part of the `update-domain` API call\.

**Note**  
To update this configuration, all apps in the domain must be in the `Stopped` or `Deleted` state\. For more information about how to stop and shut down apps, see [Shut down and Update Studio Apps](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-update-apps.html)\.

In the AWS CLI, use the following code to disable the propagation of the user profile name as the `sourceIdentity`\.

```
update-domain
 --domain-id <value>
[--default-user-settings <value>]
[--domain-settings-for-update "ExecutionRoleIdentityConfig=DISABLED"]
[--cli-input-json <value>]
[--generate-cli-skeleton <value>]
```