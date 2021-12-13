# Provision Amazon EMR Clusters from Studio<a name="configure-service-catalog-templates-studio-walkthrough"></a>

This section provides guided instructions with screenshots from Studio that show how to create a cluster, view a list of available clusters, and terminate a cluster\.

If you configured cross\-account discovery, you will see a consolidated list of clusters in Studio, and in the remote account\. 

To start, add the following permissions to the IAM execution role that will be accessing Studio\. For more information about required permissions, see [Required Permissions](studio-notebooks-emr-required-permissions.md)\.

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

After you've added the previous permissions, you can connect to Studio\. If you have an existing Studio notebook instance, you can open it\. Otherwise, if you want to create a new notebook instance, select **File**, and then select **New**\.
+ To open the cluster management page, select **Clusters** from the left\-side panel\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cluster-left-panel.png)

  The following screenshot shows the cluster management page\. From here, you can create and manage your Amazon EMR clusters\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cluster-management-page.png)

  A cluster can be in different statuses\. To filter clusters by status, select the dropdown arrow icon\. A cluster can only be in one status at a time\.

  Status options include Starting, Bootstrapping, Running/Walking, Terminating, Terminated, and Terminated with error\. The following screenshot shows the cluster status options in the status dropdown\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cluster-filter-by-status.png)

The following procedure shows how to create a cluster using a template from Studio\.

**To create a cluster from a template:**

1. Navigate to the cluster management page by selecting the **Clusters** from the left\-side panel\. Then, select **Create cluster**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/emr-create-cluster.png)

1. Enter your SageMaker Studio subnet ID, Amazon EMR cluster name, Amazon VPC ID, and SageMaker Studio security group ID\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/emr-create-cluster-details.png)

1. After entering the information from the previous step, select **Create cluster**\.

The following procedure shows how to terminate a cluster\.

**To terminate a cluster**

1. Navigate to the cluster management page by selecting the **Clusters** from the left\-side panel\.

   Select the cluster that you want to terminate, then choose **Terminate** next to **Create cluster** in the UI\. 

1. A window will appear advising that any pending work or data on your cluster will be lost after termination, and termination is irreversible\. Select **Terminate**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/emr-cluster-terminate.png)