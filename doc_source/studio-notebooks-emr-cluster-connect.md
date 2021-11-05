# Connect to an Amazon EMR Cluster from Studio<a name="studio-notebooks-emr-cluster-connect"></a>

The following instructions will explain how to connect to an Amazon EMR cluster from Studio\.with the PySpark kernel being selected\. 

1. After you have connected to Studio, you can either open an existing Studio notebook instance or can select **File** and then **New** to create a new notebook instance\. 

1. After you have an open Studio notebook instance, choose a Kernel and Instance\. Only a subset of Kernels support connecting to an Amazon EMR Cluster\. The supported images are: Data Science and SparkMagic\. The supported kernels are PySpark from the SparkMagic image and Python3\(IPython\) from the Data Science image\. Studio supports both PySpark and Scala kernels\. To switch your kernal, select in the top right of the UI the currently selected kernal and a pop\-up window will appear\. Then select a kernal of your choice from the Kernal drop\-down menu\. Lastly, select the **Select** button to make your changes\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/kernal-switcher.png)

   After you have selected your kernel of choice, select **Cluster**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cluster-button-on-toolbar-mod.png)

1. A **Connect to cluster** UI screen will appear\. Choose a cluster and select **Connect**\. Not all Amazon EMR clusters can be connected to Studio\. For more information, see the following resources:: [ Amazon EMR and Studio integration](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-emr.html) and [Perform interactive data processing using Spark in Studio Notebooks](https://aws.amazon.com/blogs/machine-learning/amazon-sagemaker-studio-notebooks-backed-by-spark-in-amazon-emr/)

   1. Connecting to a cluster will add a code block to an active cell to establish the connection\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cluster-discovery.png)

1.  If the cluster you are connecting to does not use Kerberos or LDAP connection you will be prompted to select credential type\. You can choose **HTTP basic authentication** or **No credential**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/select-auth-type.png)

1. An active cell will be populated\. This will contain all of the connection information that you need to connect to the Amazon EMR cluster that you selected\. 

   1. If the authentication type is Kerberos and HTTP Basic Auth, a widget will be created in an active cell and you will need to provide your credentials to the cluster\. Enter your **Username** and **Password**\. The following screenshot shows a successful connection after entering these credentials\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/code-injection.png)

   1. If the cluster that you are connecting to does not use Kerberos or LDAP and you selected `No credentials` you will automatically be connected to an Amazon EMR cluster\. The following screenshot shows the UI after your credentials have been successfully entered\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/successfully-connect-to-cluster-no-auth.png)

You can change the Amazon EMR cluster that the Studio Notebook is connected to by selecting **Cluster** at the top of your notebook\. To find the cluster that you want to switch to and browse the list of clusters after selecting **Cluster**in the top right corner of the UI, and sclect the cluster you want to connect to\.