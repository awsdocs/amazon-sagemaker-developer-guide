# Set Up Studio to Use Runtime IAM Roles<a name="studio-notebooks-emr-cluster-iam"></a>

To establish runtime role authentication for your Amazon EMR clusters, configure the required IAM policies, network, and usability enhancements\. Your setup depends on whether you handle any cross\-account arrangements if your Amazon EMR clusters, Amazon EMR execution role, or both, reside outside of your Amazon SageMaker Studio account\. The following discussion guides you through the policies to install, how to configure the network to allow traffic between cross\-accounts, and the local configuration file to set up to automate your Amazon EMR connection\.

## Configure runtime role authentication if your Amazon EMR cluster and Studio are in the same account<a name="studio-notebooks-emr-cluster-iam-same"></a>

If your Amazon EMR cluster resides in your Studio account, add the basic policy to connect to your Amazon EMR cluster and set permissions to call the Amazon EMR API `GetClusterSessionCredentials`, which gives you access to the cluster\. Complete the following steps to add necessary permissions to your Studio execution policy:

1. Add the required IAM policy to connect to Amazon EMR clusters\. For details, see [Discover Amazon EMR clusters from Studio](discover-emr-clusters.md)\.

1. Grant permission to call the Amazon EMR API `GetClusterSessionCredentials` when you pass one or more permitted Amazon EMR execution roles specified in the policy\.

1. \(Optional\) Grant permission to pass IAM roles that follow any user\-defined naming conventions\.

1. \(Optional\) Grant permission to access Amazon EMR clusters that are tagged with specific user\-defined strings\.

1. If you don't want to manually call the Amazon EMR connection command, install a SageMaker configuration file in your local Amazon EFS and select the role to use when you select your Amazon EMR cluster\. For details about how to preload your IAM roles, see [Preload your execution roles into Studio](#studio-notebooks-emr-cluster-iam-preload)\.

The following example policy permits Amazon EMR execution roles belonging to the modeling and training groups to call `GetClusterSessionCredentials`\. In addition, the policyholder can access Amazon EMR clusters tagged with the strings `modeling` or `training`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "elasticmapreduce:GetClusterSessionCredentials",
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "elasticmapreduce:ExecutionRoleArn": [
                        "arn:aws:iam::123456780910:role/emr-execution-role-ml-modeling*",
                        "arn:aws:iam::123456780910:role/emr-execution-role-ml-training*"
                    ],
                    "elasticmapreduce:ResourceTag/group": [
                        "*modeling*",
                        "*training*"
                    ]
                }
            }
        }
    ]
}
```

## Configure runtime role authentication if your cluster and Studio are in different accounts<a name="studio-notebooks-emr-cluster-iam-diff"></a>

If your Amazon EMR cluster is not in your Studio account, allow your Studio execution role to assume the cross\-account Amazon EMR access role so you can connect to the cluster\. Complete the following steps to set up your cross\-account configuration:

1. Create your Studio execution role permission policy so that the execution role can assume the Amazon EMR access role\. The following policy is an example:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "AllowAssumeCrossAccountEMRAccessRole",
               "Effect": "Allow",
               "Action": "sts:AssumeRole",
               "Resource": "arn:aws:iam::emr_account_id:role/emr-access-role-name"
           }
       ]
   }
   ```

1. Create the trust policy to specify which Studio account IDs are trusted to assume the Amazon EMR access role\. The following policy is an example:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "AllowCrossAccountSageMakerExecutionRoleToAssumeThisRole",
         "Effect": "Allow",
         "Principal": {
           "AWS": "arn:aws:iam::studio_account_id:role/studio_execution_role"
         },
         "Action": "sts:AssumeRole"
       }
   }
   ```

1. Create the Amazon EMR access role permission policy, which grants the Amazon EMR execution role the needed permissions to carry out the intended tasks on the cluster\. Configure the Amazon EMR access role to call the API `GetClusterSessionCredentials` with the Amazon EMR execution roles specified in the access role permission policy\. The following policy is an example:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "AllowCallingEmrGetClusterSessionCredentialsAPI",
               "Effect": "Allow",
               "Action": "elasticmapreduce:GetClusterSessionCredentials",
               "Resource": "",
               "Condition": {
                   "StringLike": {
                       "elasticmapreduce:ExecutionRoleArn": [
                           "arn:aws:iam::emr_account_id:role/emr-execution-role-name"
                       ]
                   }
               }
           }
       ]
   }
   ```

1. Set up the cross\-account network so that traffic can move back and forth between your accounts\. For guided instruction, see *Set up the network* in the blog post [Create and manage Amazon EMR Clusters from SageMaker Studio to run interactive Spark and ML workloads – Part 2](http://aws.amazon.com/blogs/machine-learning/part-2-create-and-manage-amazon-emr-clusters-from-sagemaker-studio-to-run-interactive-spark-and-ml-workloads/)\. The steps in the blog post help you complete the following tasks:

   1. VPC\-peer your Studio account and your Amazon EMR account to establish a connection\.

   1. Manually add routes to the private subnet route tables in both accounts\. This permits creation and connection of Amazon EMR clusters from the Studio account to the remote account’s private subnet\.

   1. Set up the security group attached to your Studio domain to allow outbound traffic and the security group of the Amazon EMR primary node to allow inbound TCP traffic from the Studio instance security group\.

1. If you don't want to manually call the Amazon EMR connection command, install a SageMaker configuration file in your local Amazon EFS so you can select the role to use when you choose your Amazon EMR cluster\. For details about how to preload your IAM roles, see [Preload your execution roles into Studio](#studio-notebooks-emr-cluster-iam-preload)\.

## Configure Lake Formation access<a name="studio-notebooks-emr-cluster-iam-lf"></a>

When you access data from data lakes managed by AWS Lake Formation, you can enforce table\-level and column\-level access using policies attached to your runtime role\. To configure permission for Lake Formation access, see [Integrate Amazon EMR with AWS Lake Formation](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-lake-formation.html)\.

## Preload your execution roles into Studio<a name="studio-notebooks-emr-cluster-iam-preload"></a>

If you don't want to manually call the Amazon EMR connection command, you can install a SageMaker configuration file in your local Amazon EFS so you can select the execution role to use when you choose your Amazon EMR cluster\.

To write a configuration file for the Amazon EMR execution roles, associate a [Use Lifecycle Configurations with Amazon SageMaker Studio](studio-lcc.md) \(LCC\) to the Jupyter server application\. Alternatively, you can write or update the configuration file and restart the Jupyter server with the command: `restart-jupyter-server`\.

The following snippet is an example LCC bash script you can apply if your Studio application and cluster are in the same account:

```
#!/bin/bash

set -eux

FILE_DIRECTORY="/home/sagemaker-user/.sagemaker-analytics-configuration-DO_NOT_DELETE"
FILE_NAME="emr-configurations-DO_NOT_DELETE.json"
FILE="$FILE_DIRECTORY/$FILE_NAME"

mkdir -p $FILE_DIRECTORY

cat << 'EOF' > "$FILE"
{
    "emr-execution-role-arns":
    {
      "123456789012": [
          "arn:aws:iam::123456789012:role/emr-execution-role-1",
          "arn:aws:iam::123456789012:role/emr-execution-role-2"
      ]
    }
}
EOF
```

If your Studio application and clusters are in different accounts, specify the Amazon EMR access roles that can use the cluster\. In the following example policy, *123456789012* is the ARN for the Amazon EMR cluster account, and *212121212121* and *434343434343* are the ARNs for the permitted Amazon EMR access roles\.

```
#!/bin/bash

set -eux

FILE_DIRECTORY="/home/sagemaker-user/.sagemaker-analytics-configuration-DO_NOT_DELETE"
FILE_NAME="emr-configurations-DO_NOT_DELETE.json"
FILE="$FILE_DIRECTORY/$FILE_NAME"

mkdir -p $FILE_DIRECTORY

cat << 'EOF' > "$FILE"
{
    "emr-execution-role-arns":
    {
      "123456789012": [
          "arn:aws:iam::212121212121:role/emr-execution-role-1",
          "arn:aws:iam::434343434343:role/emr-execution-role-2"
      ]
    }
}
EOF

# add your cross-account EMR access role
FILE_DIRECTORY="/home/sagemaker-user/.cross-account-configuration-DO_NOT_DELETE"
FILE_NAME="emr-discovery-iam-role-arns-DO_NOT_DELETE.json"
FILE="$FILE_DIRECTORY/$FILE_NAME"

mkdir -p $FILE_DIRECTORY

cat << 'EOF' > "$FILE"
{
    "123456789012": "arn:aws:iam::123456789012:role/cross-account-emr-access-role"
}
EOF
```