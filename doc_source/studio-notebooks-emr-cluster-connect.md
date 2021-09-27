# Connect to an Amazon EMR Cluster from Studio<a name="studio-notebooks-emr-cluster-connect"></a>

The next set of instructions will explain how to connect to an Amazon EMR Cluster from Studio\.

1. Once connected to Studio, you can either open an existing Studio notebook instance or can select File and then New to create a new notebook instance\. 

1. After you have an open Studio notebook instance, choose a Kernel and Instance\. Only a subset of Kernels support connecting to an Amazon EMR Cluster\. The supported images are: Data Science and SparkMagic\. The supported kernels are PySpark from SparkMagic image and Python3\(IPython\) from Data Science image\. You will see a Cluster button appear in the top right corner of the notebook instance\. Select **Cluster**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cluster-button-on-toolbar-mod.png)

1. A **Connect to cluster** UI screen will appear\. Choose a cluster and select **Connect**\. Not all Amazon EMR clusters can be connected to Studio\. For more information, see the following resources:: [Studio/Amazon EMR integration](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-emr.html) and [Studio Amazon EMR integration blog post\. ](https://aws.amazon.com/blogs/machine-learning/amazon-sagemaker-studio-notebooks-backed-by-spark-in-amazon-emr/)

   1. Connecting to a cluster will add a code block to an active cell to establish the connection\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/cluster-discovery.png)

1.  If the cluster you are connecting to does not use Kerberos or LDAP connection then you will be prompted to select credential type\. You can choose HTTP basic authentication or No credential\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/select-auth-type.png)

1. An active cell will be populated with all the connection information needed to connect to the Amazon EMR cluster you selected earlier\. 

   1. If the authentication type is Kerberos and HTTP Basic Auth, a widget will be created in an active cell and you will need to provide your credentials to the cluster\. Enter your **Username** and **Password**\. The following is a screenshot showing a successful connection after entering these credentials to a cluster\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/code-injection.png)

   1. If the cluster you are connecting to does not use Kerberos or LDAP and you selected `No credentials` then you will automatically be connected to an Amazon EMR cluster\. The following is a screenshot showing this\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/successfully-connect-to-cluster-no-auth.png)

You can change the Amazon EMR cluster that the Studio Notebook is connected to by selecting **Cluster** at the top of your notebook\. Then simply browse to find the cluster you want to switch to and click to connect to it\.