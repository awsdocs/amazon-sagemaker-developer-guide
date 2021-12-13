# Connect to an Amazon EMR Cluster from Studio<a name="studio-notebooks-emr-cluster-connect"></a>

This guide explains how you can connect to an Amazon EMR cluster from SageMaker Studio with the PySpark kernel selected\. 

**To connect Amazon EMR cluster with PySpark kernel selected**

1. After you connect to Studio, if you have an existing Studio notebook instance, open that\. Otherwise, to create a new notebook instance, select **File**, and then select **New**\. 

1. After you have an open Studio notebook instance, choose a kernel and instance\. 
**Note**  
Only a subset of kernels can connect to an Amazon EMR cluster\. The supported images are Data Science and SparkMagic\. The supported kernels are PySpark from the SparkMagic image and Python3 \(IPython\) from the Data Science image\. Studio supports both PySpark and Scala kernels\. 

   To switch your kernel, select in the top right of the UI the currently selected kernel where a pop\-up window appears\. Then select a kernel of your choice from the kernel drop\-down menu\. Lastly, select the **Select** button to make your changes\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/kernal-switcher.png)

1. After you have selected your kernel of choice, select **Cluster**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cluster-button-on-toolbar-mod.png)

1. A **Connect to cluster** UI screen will appear\. Choose a cluster and select **Connect**\. Not all Amazon EMR clusters can be connected to Studio\. For more information, see [Perform interactive data processing using Spark in Studio Notebooks](http://aws.amazon.com/blogs/machine-learning/amazon-sagemaker-studio-notebooks-backed-by-spark-in-amazon-emr/)

   1. When you connect to a cluster, it adds a code block to an active cell to establish the connection\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cluster-discovery.png)

1. If the cluster that you're connecting to does not use Kerberos or Lightweight Directory Access Protocol \(LDAP\) connection, you will be prompted to select the credential type\. You can choose **HTTP basic authentication** or **No credential**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/select-auth-type.png)

1. An active cell will populate\. This will contain the connection information that you need for connecting to the Amazon EMR cluster that you selected\. 

   1. When the authentication type is Kerberos and HTTP Basic Auth, a widget will be created in an active cell for you to provide your **Username** and **Password**\. The following screenshot shows a successful connection after entering these credentials\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/code-injection.png)

   1. If the cluster that you are connecting to does not use Kerberos or LDAP, and you selected `No credentials`, you will automatically connect to an Amazon EMR cluster\. The following screenshot shows the UI after credentials have been successfully entered\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/successfully-connect-to-cluster-no-auth.png)

1. 
   + This step is optional\. If you want to change the Amazon EMR cluster that the Studio notebook is connected to, select **Cluster** at the top\-right of your notebook\. After selecting **Cluster**, browse the list of clusters and select a different cluster\.

For more information on required permissions, see [Required Permissions](studio-notebooks-emr-required-permissions.md)\.

## Connect Amazon EMR Clusters Across Accounts<a name="connect-emr-clusters-across-accounts"></a>

If you have set up cross\-account discoverability and connectivity, when you select **Cluster**, all clusters from both Studio and remote accounts will show\. After you select **Connect**, Studio will initiate and establish a connection to the Amazon EMR cluster in the remote account\. The following screenshot shows this connection\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cross-account-connectivity.png)