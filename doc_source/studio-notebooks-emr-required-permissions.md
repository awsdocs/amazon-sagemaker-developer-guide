# Required Permissions<a name="studio-notebooks-emr-required-permissions"></a>

This guide shows you how to apply an example IAM policy to your execution role\. The examples in this guide provide you with various permissions and monitoring capabilities\. At the end of this guide, you will find a comprehensive IAM policy role that includes all required permissions\. These permissions provide access to Amazon EMR clusters from within Studio, and allow discoverability of clusters from within Studio \(for presigned URLs\)\. Access the [IAM console](https://console.aws.amazon.com/iam) to add the policy to your role\.

To discover and connect to Amazon EMR clusters from within Studio, ensure that your execution role has the following permissions:

```
{
            "Sid": "AllowClusterDetailsDiscovery",
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:DescribeCluster",
                "elasticmapreduce:ListInstanceGroups"
            ],
            "Resource": [
                "arn:aws:elasticmapreduce:<region>:<account-id>:cluster/*"
            ]
        },
        {
            "Sid": "AllowClusterDiscovery",
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:ListClusters"
            ],
            "Resource": "*"
}
```

To enhance monitoring capabilities, attach the following permissions to your Studio execution role:

```
{
            "Sid": "AllowPresignedUrl",
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:DescribeCluster",
                "elasticmapreduce:ListInstanceGroups",
                "elasticmapreduce:CreatePersistentAppUI",
                "elasticmapreduce:DescribePersistentAppUI",
                "elasticmapreduce:GetPersistentAppUIPresignedURL",
                "elasticmapreduce:GetOnClusterAppUIPresignedURL"
            ],
            "Resource": [
                "arn:aws:elasticmapreduce:<region>:<account-id>:cluster/*"
            ]
}
```

To allow users to create Amazon EMR clusters, add the following permission to your Studio execution role:

```
{
            "Sid": "AllowSagemakerProjectManagement",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateProject",
                "sagemaker:DeleteProject"
            ],
            "Resource": "arn:aws:sagemaker:<region>:<account-id>:project/*"
}
```

If you have provided Studio with a cross\-account discovery and connectivity role, the role in the non\-Studio account must allow "assume role" permissions to the Studio execution role\. This can be done by attaching the following permissions to your Studio execution role:

```
{ 
            "Sid": "AllowRoleAssumptionForCrossAccountDiscovery", 
            "Effect": "Allow", 
            "Action": "sts:AssumeRole", 
            "Resource": ["arn:aws:iam::<cross-account>:role/<studio-execution-role>" ]
}
```

To allow discoverability for Amazon EMR templates, attach the following permissions to your Studio execution role: 

```
{
            "Sid": "AllowEMRTemplateDiscovery",
            "Effect": "Allow",
            "Action": [
              "servicecatalog:SearchProducts"
            ],
            "Resource": "*"
}
```

The following is a comprehensive example IAM policy that includes all of the previous permissions in this guide:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowPresignedUrl",
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:DescribeCluster",
                "elasticmapreduce:ListInstanceGroups",
                "elasticmapreduce:CreatePersistentAppUI",
                "elasticmapreduce:DescribePersistentAppUI",
                "elasticmapreduce:GetPersistentAppUIPresignedURL",
                "elasticmapreduce:GetOnClusterAppUIPresignedURL"
            ],
            "Resource": [
                "arn:aws:elasticmapreduce:<region>:<account-id>:cluster/*"
            ]
        },
        {
            "Sid": "AllowClusterDetailsDiscovery",
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:DescribeCluster",
                "elasticmapreduce:ListInstanceGroups"
            ],
            "Resource": [
                "arn:aws:elasticmapreduce:<region>:<account-id>:cluster/*"
            ]
        },
        {
            "Sid": "AllowClusterDiscovery",
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:ListClusters"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowSagemakerProjectManagement",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateProject",
                "sagemaker:DeleteProject"
            ],
            "Resource": "arn:aws:sagemaker:<region>:<account-id>:project/*"
        },
        { 
            "Sid": "AllowRoleAssumptionForCrossAccountDiscovery", 
            "Effect": "Allow", 
            "Action": "sts:AssumeRole", 
            "Resource": ["arn:aws:iam::<cross-account>:role/<studio-execution-role>" ]
        },
        {
            "Sid": "AllowEMRTemplateDiscovery",
            "Effect": "Allow",
            "Action": [
              "servicecatalog:SearchProducts"
            ],
            "Resource": "*"
        }
    ]
}
```