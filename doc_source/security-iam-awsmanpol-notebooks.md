# AWS Managed Policies for SageMaker Notebooks<a name="security-iam-awsmanpol-notebooks"></a>

These AWS managed policies add permissions required to use SageMaker Notebooks\. The policies are available in your AWS account and are used by execution roles created from the SageMaker console\.

**Topics**
+ [AWS managed policy: AmazonSageMakerNotebooksServiceRolePolicy](#security-iam-awsmanpol-AmazonSageMakerNotebooksServiceRolePolicy)
+ [Amazon SageMaker updates to SageMaker Notebooks managed policies](#security-iam-awsmanpol-notebooks-updates)

## AWS managed policy: AmazonSageMakerNotebooksServiceRolePolicy<a name="security-iam-awsmanpol-AmazonSageMakerNotebooksServiceRolePolicy"></a>

This AWS managed policy grants permissions commonly needed to use Amazon SageMaker Notebooks\. The policy is added to the `AmazonSageMaker-ExecutionRole` that is created when you onboard to Amazon SageMaker Studio\. For more information on service\-linked roles, see [Service\-Linked Roles](security_iam_service-with-iam.md#security_iam_service-with-iam-roles-service-linked)\.

**Permissions details**

This policy includes the following permissions\.
+ `elasticfilesystem` – Allows principals to create and delete Amazon Elastic File System \(EFS\) file systems, access points, and mount targets\. These are limited to those tagged with the key *ManagedByAmazonSageMakerResource*\. Allows principals to describe all EFS file systems, access points, and mount targets\. Allows principals to create or overwrite tags for EFS access points and mount targets\.
+ `ec2` – Allows principals to create network interfaces and security groups for Amazon Elastic Compute Cloud \(EC2\) instances\. Also allows principals to create and overwrite tags for these resources\.
+ `sso` – Allows principals to add and delete managed application instances to AWS IAM Identity Center \(successor to AWS Single Sign\-On\)\.
+ `sagemaker` – Allows principals to create and read SageMaker user profiles\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "elasticfilesystem:CreateAccessPoint",
            "Resource": "arn:aws:elasticfilesystem:*:*:file-system/*",
            "Condition": {
                "StringLike": {
                    "aws:ResourceTag/ManagedByAmazonSageMakerResource": "*",
                    "aws:RequestTag/ManagedByAmazonSageMakerResource": "*"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticfilesystem:DeleteAccessPoint"
            ],
            "Resource": "arn:aws:elasticfilesystem:*:*:access-point/*",
            "Condition": {
                "StringLike": {
                    "aws:ResourceTag/ManagedByAmazonSageMakerResource": "*"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "elasticfilesystem:CreateFileSystem",
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "aws:RequestTag/ManagedByAmazonSageMakerResource": "*"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticfilesystem:CreateMountTarget",
                "elasticfilesystem:DeleteFileSystem",
                "elasticfilesystem:DeleteMountTarget"
            ],
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "aws:ResourceTag/ManagedByAmazonSageMakerResource": "*"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticfilesystem:DescribeAccessPoints",
                "elasticfilesystem:DescribeFileSystems",
                "elasticfilesystem:DescribeMountTargets"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "elasticfilesystem:TagResource",
            "Resource": [
                "arn:aws:elasticfilesystem:*:*:access-point/*",
                "arn:aws:elasticfilesystem:*:*:file-system/*"
            ],
            "Condition": {
                "StringLike": {
                    "aws:ResourceTag/ManagedByAmazonSageMakerResource": "*"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "ec2:CreateTags",
            "Resource": [
                "arn:aws:ec2:*:*:network-interface/*",
                "arn:aws:ec2:*:*:security-group/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateNetworkInterface",
                "ec2:CreateSecurityGroup",
                "ec2:DeleteNetworkInterface",
                "ec2:DescribeDhcpOptions",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcs",
                "ec2:ModifyNetworkInterfaceAttribute"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:AuthorizeSecurityGroupEgress",
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:CreateNetworkInterfacePermission",
                "ec2:DeleteNetworkInterfacePermission",
                "ec2:DeleteSecurityGroup",
                "ec2:RevokeSecurityGroupEgress",
                "ec2:RevokeSecurityGroupIngress"
            ],
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "ec2:ResourceTag/ManagedByAmazonSageMakerResource": "*"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "sso:CreateManagedApplicationInstance",
                "sso:DeleteManagedApplicationInstance",
                "sso:GetManagedApplicationInstance"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateUserProfile",
                "sagemaker:DescribeUserProfile"
            ],
            "Resource": "*"
        }
    ]
}
```

## Amazon SageMaker updates to SageMaker Notebooks managed policies<a name="security-iam-awsmanpol-notebooks-updates"></a>

View details about updates to AWS managed policies for Amazon SageMaker since this service began tracking these changes\.


| Policy | Version | Change | Date | 
| --- | --- | --- | --- | 
|  [AmazonSageMakerNotebooksServiceRolePolicy](#security-iam-awsmanpol-AmazonSageMakerNotebooksServiceRolePolicy)  | 7 |  Added `elasticfilesystem:TagResource` permission\.  | March 9, 2023 | 
| AmazonSageMakerNotebooksServiceRolePolicy | 6 |  Added permissions for `elasticfilesystem:CreateAccessPoint`, `elasticfilesystem:DeleteAccessPoint`, and `elasticfilesystem:DescribeAccessPoints`\.  | January 12, 2023 | 
|  |  |  SageMaker started tracking changes for its AWS managed policies\.  | June 1, 2021 | 