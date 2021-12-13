# Troubleshoot and Monitor Workloads in Amazon EMR<a name="studio-notebooks-emr-cluster-trouble-shoot"></a>

The following sections give instructions for accessing the Spark UI from SageMaker Studio notebooks\. The Spark UI allows you to monitor and debug your Spark Jobs submitted to run on Amazon EMR from Studio notebooks\. SSH tunneling and presigned URLs are two ways for accessing the Spark UI\.

## Set up SSH tunneling for Spark UI access<a name="studio-notebooks-emr-ssh-tunneling"></a>

To set up SSH tunneling to access the Spark UI, follow one of the two options in this section\. Note that the screenshot in Step 6b of [Connect to an Amazon EMR Cluster from Studio](studio-notebooks-emr-cluster-connect.md) shows links under Spark UI and Driver log\. These links will activate only after you complete the SSH tunneling setup\.

Options for setting up SSH tunneling:
+ [Option 1: Set up an SSH tunnel to the master node using local port forwarding](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel-local.html)
+ [Option 2, part 1: Set up an SSH tunnel to the master node using dynamic port forwarding](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel.html)
  + [Option 2, part 2: Configure proxy settings to view websites hosted on the master node](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-master-node-proxy.html)

For information about viewing web interfaces hosted on Amazon EMR clusters, see [View web interfaces hosted on Amazon EMR Clusters](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-web-interfaces.html)\. You can also visit your Amazon EMR console to get access to the Spark UI\.

**Note**  
You can set up an SSH tunnel even if presigned URLs are not available to you\. 

## Presigned URLs<a name="troubleshoot-monitor-workloads-presigned-urls"></a>

To create one\-click URLs that can access Spark UI on Amazon EMR from SageMaker Studio notebooks, you must enable the following IAM permissions\. Choose the option that applies to you: 
+ **For Amazon EMR clusters that are in the same account as the SageMaker Studio notebook: Add the following permissions to the SageMaker Studio IAM execution role\. **
+ **For Amazon EMR clusters that are in a different account \(not SageMaker Studio notebook\): Add the following permissions to the cross\-account role that you created for [Discover Amazon EMR Clusters from Studio](emr-cluster-discover.md)\.**

**Note**  
You can access presigned URLs from the console in the following regions:  
US East \(N\. Virginia\) Region
US West \(N\. California\) Region
Canada \(Central\) Region
Europe \(Frankfurt\) Region
Europe \(Stockholm\) Region
Europe \(Ireland\) Region
Europe \(London\) Region
Europe \(Paris\) Region
Asia Pacific \(Tokyo\) Region
Asia Pacific \(Seoul\) Region
Asia Pacific \(Sydney\) Region
Asia Pacific \(Mumbai\) Region
Asia Pacific \(Singapore\) Region
South America \(São Paulo\)

 The following policy gives access to presigned URLs for your execution role\. 

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

For more information about required permissions, see [Required Permissions](studio-notebooks-emr-required-permissions.md)\.