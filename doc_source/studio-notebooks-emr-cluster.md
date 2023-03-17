# Prepare Data using Amazon EMR<a name="studio-notebooks-emr-cluster"></a>

Data scientists and data engineers use Apache Spark, Hive, and Presto on [Amazon EMR](http://aws.amazon.com/emr) for fast data preparation\. Studio comes with built\-in integration of Amazon EMR, enabling you to perform petabyte\-scale interactive data preparation and machine learning right in your Studio notebook\. Within your notebook, you can visually browse, discover, and connect to Amazon EMR\. After you connect, you can interactively explore, visualize, and prepare petabyte\-scale data for machine learning \(ML\) using Apache Spark, Hive, and Presto\. Amazon EMR can handle your ETL jobs, run large\-scale model training, perform analyses, and handle reporting, among many other capabilities\.

For guided instructions about how to connect to an Amazon EMR cluster from SageMaker Studio, see [Create and manage Amazon EMR Clusters from SageMaker Studio to run interactive Spark and ML workloads](http://aws.amazon.com/blogs/machine-learning/part-1-create-and-manage-amazon-emr-clusters-from-sagemaker-studio-to-run-interactive-spark-and-ml-workloads/)\.

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
+ SageMaker Studio comes with built\-in support to connect to EMR clusters in the following images and kernels:
  + Images: Data Science, Data Science 2\.0, SparkMagic, SparkAnalytics 1\.0, PyTorch 1\.8, TensorFlow 2\.6
  + Kernel: PySpark and Spark kernels for the SparkMagic image, Python 3 \(IPython\) for the Data Science, Data Science 2\.0, PyTorch 1\.8, TensorFlow 2\.6 images\.
+ If you want to connect to EMR clusters using another built\-in image or your own image, follow instructions in [Bring your own image](#studio-notebooks-emr-process-byoi)\.

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
+ [Discover Amazon EMR clusters from Studio](discover-emr-clusters.md)
+ [Connect to a cluster from Studio](studio-notebooks-emr-cluster-connect.md)
+ [Troubleshoot and Monitor Workloads in Amazon EMR](studio-notebooks-emr-cluster-trouble-shoot.md)
+ [Manage Amazon EMR Clusters from Studio](manage-emr-clusters-from-studio.md)
+ [Required Permissions](studio-notebooks-emr-required-permissions.md)