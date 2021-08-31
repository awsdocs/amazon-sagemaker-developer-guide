# Set Up a Connection to an Amazon EMR Cluster<a name="studio-emr"></a>

Amazon EMR is a big data platform for processing vast amounts of data\. The central component of Amazon EMR is the cluster\. A cluster is a collection of Amazon EC2 instances\. Apache Spark is a distributed processing framework that runs on Amazon EMR\. For more information, see [What Is Amazon EMR?](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html) and [Apache Spark](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-spark.html)\.

Amazon SageMaker Studio comes with a SageMaker SparkMagic image that contains a PySpark kernel\. The SparkMagic image also contains an AWS CLI utility, `sm-sparkmagic`, that you can use to create the configuration files required for the PySpark kernel to connect to the Amazon EMR cluster\. After creating the configuration files, the utility displays the steps required to finish the setup\.

For added security, you can specify that the connection to the EMR cluster uses Kerberos authentication\. For more information, see [Use Kerberos Authentication](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-kerberos.html)\.

**Prerequisites**
+ Access to SageMaker Studio that's set up to use Amazon VPC mode\. To connect to Amazon EMR, Studio must be configured as Amazon VPC only mode\. For more information, see [Connect SageMaker Studio Notebooks to Resources in a VPC](studio-notebooks-and-internet-access.md)\. All subnets used by SageMaker Studio must be private subnets\.
+ If you use the `sm-sparkmagic` utility to configure the sparkmagic kernel, ensure that the VPC interface endpoint is attached to all of the subnets used by SageMaker Studio or ensure that all of the subnets used by SageMaker Studio are routed to use a NAT gateway\. For more information, see [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)\.
+ An Amazon EMR cluster in the same VPC as Studio or in a VPC that's connected to the same VPC as Studio\. This cluster must have Spark and Livy installed\.
+ The security group used for Amazon SageMaker Studio and the Amazon EMR security group must allow access to and from each other\. 
+ Your Amazon EMR security group must open port 8998, so Amazon SageMaker Studio can communicate with the Spark cluster via Livy\. For more information on setting up the security group, see [Build SageMaker notebooks backed by Spark in Amazon EMR](http://aws.amazon.com/blogs/machine-learning/build-amazon-sagemaker-notebooks-backed-by-spark-in-amazon-emr/)\.
+ If you use the `sm-sparkmagic` utility, the IAM execution role associated with your Studio user profile must contain the following extra permissions\. To find the execution role, choose your user name in the SageMaker Studio Control Panel\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "elasticmapreduce:DescribeCluster",
                  "elasticmapreduce:DescribeSecurityConfiguration",
                  "elasticmapreduce:ListInstances"
              ],
              "Resource": [
                  "arn:aws:elasticmapreduce:*:*:cluster/*"
              ]
          }
      ]
  }
  ```

**To set up a connection to an EMR cluster**

1. Open SageMaker Studio\.

1. In the upper\-left corner of Studio, choose **Amazon SageMaker Studio** to open Studio Launcher\.

1. On the **Launcher** page, choose **Notebooks and compute resources**\.

1. For **Select a SageMaker image**, choose the **SparkMagic** image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-spark-select.png)

1. Choose **Notebook** to create a Studio notebook in the SparkMagic image\.

1. Run the following code in a notebook cell to create the configuration files used to connect to the EMR cluster\. `%%local` ensures that the code runs in the local image instead of on Spark\.
**Note**  
The `sm-sparkmagic` utility does not support generating configuration files when your Amazon EMR cluster and Studio Domain are using two different Amazon VPCs and the Amazon VPCs have different `DHCPOptions`\.
   + If the EMR cluster is not configured for Kerberos authentication, run the following command:

     ```
     %%local
     ! sm-sparkmagic connect --cluster-id "cluster-id"
     ```

     The output should be similar to the following:

     ```
     Successfully read emr cluster(cluster-id) details
     SparkMagic config file location: /etc/sparkmagic/config.json
     ```
   + If the EMR cluster is configured for Kerberos authentication, run the following command:

     ```
     ! sm-sparkmagic connect --cluster-id "cluster-id" --user-name "user-name"
     ```

     The output should be similar to the following:

     ```
     Successfully read emr cluster(cluster-id) details
     SparkMagic config file location: /etc/sparkmagic/config.json
     Kerberos configuration file location: /etc/krb5.conf
     ```

1. To complete the setup, do one of the following: 
   + For EMR clusters that are not configured for Kerberos authentication, go to step 8\.
   + For EMR clusters that are configured for Kerberos authentication, do the following:

     1. In the notebook toolbar, choose the **Launch terminal** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/notebook-launch-terminal-small.png)\) to open a terminal in the same SparkMagic image as the notebook\.

     1. Run the following command in the terminal to get the Kerberos ticket:

        ```
        kinit user-name
        ```

     1. Enter your password when prompted\.

1. In the notebook toolbar, choose the **Restart kernel** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/notebook-restart-kernel-small.png)\) to complete the setup\.

1. To verify that the connection was set up correctly, run the following command in a notebook cell:

   ```
   %%info
   ```

   The output should be similar to the following:

   ```
   Current session configs:{'driverMemory': '1000M', 'executorCores': 2, 'kind': 'pyspark'}
   No active sessions.
   ```

**For more information**
+ [Perform interactive data processing using Spark in Amazon SageMaker Studio Notebooks](http://aws.amazon.com/blogs/machine-learning/amazon-sagemaker-studio-notebooks-backed-by-spark-in-amazon-emr/)\.