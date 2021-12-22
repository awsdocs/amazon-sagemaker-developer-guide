# Discover Amazon EMR Clusters from Studio<a name="discover-emr-clusters"></a>

From within Studio, data scientists and data engineers can easily discover, connect to, and manage Amazon EMR clusters\. Your Amazon EMR clusters may be in the same AWS account as SageMaker Studio or they may be in a different AWS account\. This section explains how to discover an Amazon EMR cluster that exists in the same account as SageMaker Studio\. For information about discovering Amazon EMR clusters in a different AWS account, see [Discover Amazon EMR Clusters Across Accounts](#discover-emr-clusters-across-accounts)\.

Before you can activate discovering functionality, you must add a required policy to your Studio execution role\. After you add this policy, you can connect to an Amazon EMR cluster in Studio\. 

**To add the required policy to your Studio execution role**

1. Open the [IAM console](https://console.aws.amazon.com/iam/)\.

1. Select **Roles** in the left\-side panel\.

1. Find the Studio execution role that you will be using and select it\. 

1. Under the **Permissions **tab, select **Attach policies**\.

1. Select **Create policy**\.

1. Select **JSON**\.

1. Copy and paste in the following policy\. For more information on required permissions, see [Required Permissions](studio-notebooks-emr-required-permissions.md)\.

   ```
   {
       "Statement":
       [
           {
               "Action":
               [
                   "elasticmapreduce:DescribeCluster",
                   "elasticmapreduce:ListInstanceGroups"
               ],
               "Effect": "Allow",
               "Resource":
               [
                   "arn:aws:elasticmapreduce:*:*:cluster/*"
               ]
           },
           {
               "Action":
               [
                   "elasticmapreduce:ListClusters"
               ],
               "Effect": "Allow",
               "Resource": "*"
           }
       ],
       "Version": "2012-10-17"
   }
   ```

1. Select **Next: Tags**\.

1. Select **Next: Review**\.

1. Enter a **Name**, **Description**, and Select **Create policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/step10-create-policy.png)

1. Now you can log in to Studio\. If this is your first time logging in, follow the instructions in [ Log in to Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks-get-started.html#notebooks-get-started-log-in)\.

## Discover Amazon EMR Clusters Across Accounts<a name="discover-emr-clusters-across-accounts"></a>

If you want to discover Amazon EMR clusters in a different AWS account than the SageMaker Studio is in, you must specify the IAM role ARN in the remote account\. This IAM role ARN must be assumed to list and describe Amazon EMR clusters\. Specify this remote role in a file that's named `emr-discovery-iam-role-arns-DO_NOT_DELETE.json`, and in a directory named `.cross-account-configuration-DO_NOT_DELETE`\. You will find this in your home directory, located in the Amazon EFS Storage volume used by SageMaker Studio\. You can automate this process by using Lifecycle Configuration \(LCC\) scripts\. You can attach the LCC to your Studio domain or user profile\. You have the option to set your LCC script to run by default when your Jupyter server starts\.

The LCC script that you use must be a JupyterServer configuration\. For more information on how to create and use your LCC script and how to attach it at the `Domain` and `UserProfile` level, see [Use Lifecycle Configurations with Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-lcc.html)\. For more information on required permissions, see [Required Permissions](studio-notebooks-emr-required-permissions.md)\.

1. The following is an example LCC script that you can use\. To modify the script, populate the script with the following details\. Make sure that you replace `ASSUMABLE-ROLE` and 123456789012 with your role name and account ID, respectively\. There is a limit of one role name and account ID combination\. 

   ```
   # This script creates the file that informs SageMaker Studio that the role "arn:aws:iam::123456789012:role/ASSUMABLE-ROLE" in remote account "123456789012" must be assumed to list and describe EMR clusters in the remote account.
   
   #!/bin/bash
   
   set -eux
   
   FILE_DIRECTORY="/home/sagemaker-user/.cross-account-configuration-DO_NOT_DELETE"
   FILE_NAME="emr-discovery-iam-role-arns-DO_NOT_DELETE.json"
   FILE="$FILE_DIRECTORY/$FILE_NAME"
   
   mkdir -p $FILE_DIRECTORY
   
   cat > "$FILE" <<- "EOF"
   {
     "123456789012": "arn:aws:iam::123456789012:role/ASSUMABLE-ROLE"
   }
   EOF
   ```