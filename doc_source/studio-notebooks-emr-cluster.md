# Prepare Data at Scale with Studio Notebooks<a name="studio-notebooks-emr-cluster"></a>

Studio gives data scientists, machine learning \(ML\) engineers, and general practitioners the tools to perform data analytics and data preparation at scale\. From within a Studio notebook, you can visually browse, discover, and connect to [Amazon EMR](http://aws.amazon.com/emr)\. After you’re connected, you can interactively explore, visualize, and prepare petabyte\-scale data for machine learning \(ML\) using Apache Spark, Hive, and Presto\. 

Analyzing, transforming, and preparing large amounts of data is a foundational step of any data science and ML workflow\. Running interactive analytics and data preparation on Amazon EMR and SageMaker Studio notebooks can serve as a unified environment for complete data science and data engineering workflows\. 

Studio also supports a tool to share your notebook with colleagues for collaboration through the UI\. With this capability, you can now build ML workflows directly from Studio notebooks\. Connecting to an Amazon EMR cluster using SageMaker Studio can also help improve team efficiency by streamlining the setup for ML workflows\. 

The supported images and kernels for connecting to an Amazon EMR cluster are as follows:
+ Images: Data Science, SparkMagic, PyTorch 1\.8, TensorFlow 2\.8
+ Kernel: PySpark and Spark kernels for the SparkMagic image under running apps, and Python 3 \(IPython\) for the Data Science image\.

For guided instructions on how to connect to an Amazon EMR cluster from Studio, see [ Perform interactive data engineering and data science workflows from SageMaker Studio notebooks](http://aws.amazon.com/blogs/machine-learning/perform-interactive-data-engineering-and-data-science-workflows-from-amazon-sagemaker-studio-notebooks/)\.

For detailed information about required permissions, see [Required Permissions](studio-notebooks-emr-required-permissions.md)\.

**Prerequisites**
+ You will need access to SageMaker Studio that is set up to use Amazon Virtual Private Cloud \(Amazon VPC\) mode\. 
  + To connect to Amazon EMR, Studio must be configured as Amazon VPC only mode\. For more information, see [Connect SageMaker Studio Notebooks to Resources in a Amazon VPC](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-notebooks-and-internet-access.html)\. For more information on how to onboard, see [Onboard to SageMaker Domain](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html)\.
+ All subnets used by SageMaker Studio must be private subnets\. 
+ If you use the `sm-analytics` utility to configure the SparkMagic kernel, follow one of these two prerequisites:
  + Make sure that the Amazon VPC interface endpoint is attached to all of the subnets used by SageMaker Studio\.
  + Ensure that all of the subnets used by SageMaker Studio are routed to use a NAT gateway\. For more information, see [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)\.
+ If either one of the following points apply to you, you must have Spark and Livy installed when using Amazon EMR\.
  + Your Amazon EMR cluster is in the same Amazon VPC as Studio\.
  + Your cluster is in a Amazon VPC that's connected to the Amazon VPC in Studio\.
+ The security groups for both Amazon SageMaker Studio and Amazon EMR must allow access to and from each other\. 
+ Your Amazon EMR security group must open port 8998, so that Amazon SageMaker Studio can communicate with the Spark cluster through Livy\. For more information about setting up the security group, see [Build SageMaker notebooks backed by Spark in Amazon EMR](http://aws.amazon.com/blogs/machine-learning/build-amazon-sagemaker-notebooks-backed-by-spark-in-amazon-emr/)\. 
+ To connect to an Amazon EMR cluster from Studio, you must first access SageMaker Studio\. If you have not set up SageMaker Studio, follow the [Get Started guide](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks-get-started.html)\.
+ If you created a new domain during Studio setup, then discovering an Amazon EMR cluster from Studio should be available to you\. 
  + If you are reusing an existing domain, you must update both Studio and Studio applications\. For detailed instructions, see [Update Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-update-studio.html) and [Update Studio applications ](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-update-apps.html)\. 

## Bring your own image<a name="studio-notebooks-emr-process-byoi"></a>

If you want to bring your own image, first install the following dependencies to your kernel\. The following list shows `pip` commands with the library name that you will install\.

```
pip install sparkmagic
pip install sagemaker-studio-sparkmagic-lib
pip install sagemaker-studio-analytics-extension
```

You can update the libraries from the previous list manually, if they are not the latest version\. 

If you want to connect to Amazon EMR with Kerberos authentication, you must install the kinit client\. Depending on your OS, the command to install the kinit client can vary\. To bring an Ubuntu \(Debian based\) image, use the `apt-get install -y -qq krb5-user` command\.

**Topics**
+ [Bring your own image](#studio-notebooks-emr-process-byoi)
+ [Discover Amazon EMR Clusters from Studio](discover-emr-clusters.md)
+ [Connect to an Amazon EMR Cluster from Studio](studio-notebooks-emr-cluster-connect.md)
+ [Troubleshoot and Monitor Workloads in Amazon EMR](studio-notebooks-emr-cluster-trouble-shoot.md)
+ [Manage Amazon EMR Clusters from Studio](manage-emr-clusters-from-studio.md)
+ [Required Permissions](studio-notebooks-emr-required-permissions.md)