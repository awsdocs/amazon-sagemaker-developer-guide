# Discover Amazon EMR Clusters from Studio<a name="emr-cluster-discover"></a>

To connect to an Amazon EMR cluster from Studio you will first need to access SageMaker Studio\. If you have not yet set up SageMaker Studio then follow the [Get Started guide](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks-get-started.html)\.

If you created a new domain in the setup of Studio then this new feature should be available to you\. If you are reusing an existing domain you will need to update both Studio and Studio Apps\. See, [Update Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-update-studio.html) and [Update Studio Apps ](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-update-apps.html) for detailed information on how to do this\. 

To connect to an Amazon EMR cluster in Studio, you will need to enable the discovering functionality by adding a required policy to your Studio execution role\. The following steps explain how to do this\.

1. Open the [IAM console](https://console.aws.amazon.com/iam/)\.

1. Select **Roles** in the left panel\.

1. Find the Studio execution role you will be using and select it\. 

1. Under the **Permissions **tab, select **Attach policies**\.

1. Select **Create policy**\.

1. Select **JSON**\.

1. Copy and paste in the following policy:

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

1. Enter a Name, Description, and Select **Create policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/step10-create-policy.png)

1. Next you will need to log into Studio\. If this is your first time, then follow these instructions to [ Log into Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks-get-started.html#notebooks-get-started-log-in)\.