# Prepare Data at Scale with Studio Notebooks<a name="studio-notebooks-emr-cluster"></a>

 Amazon SageMaker Studio enables data scientists, machine learning engineers, and general practitioners to discover and connect to Amazon EMR clusters from within Studio\. After you have connected to an Amazon EMR cluster from Studio, you will be able to interactively explore and query data\. When using Apache Spark from within SageMaker Studio notebooks, you can reprocess and prepare large amounts of data for analysis and model training\. 

SageMaker Studio supports analytic capabilities by connecting to an Amazon EMR cluster\. This provides you with a unified notebook experience for analytics and machine learning \(ML\)\. When using Spark workloads or SageMaker Processing jobs, you can train, debug, and deploy ML models, and run data processing and feature engineering jobs from a single notebook instance\.

Studio also supports a tool to share your notebook with colleagues for collaboration through the UI\. With this capability, you can now build end\-to\-end ML workflows directly from within Studio notebooks\. Connecting to an Amazon EMR cluster using SageMaker Studio can also help improve team effeciency by streamlining the setup for ML workflows\. 

Your SageMaker Studio account allows you to browse a list of Amazon EMR clusters and click to connect to a remote cluster\. The Studio UI automatically injects a command into your notebook to initiate connection with the remote cluster using Kerberos, HTTP, or non\-auth authentication methods\.

The supported images and kernels for connecting to an Amazon EMR cluster are as follows:
+ Images: Data Science, SparkMagic, PyTorch 1\.8, TensorFlow 2\.3
+ Kernel: PySpark and Spark kernels for the SparkMagic image under running apps, and Python 3 \(IPython\) for the Data Science image\.

The Sparkmagic display name has been changed from sparkmagic\-1\.0 to sagemaker\-sparkmagic\. The current application name is sagemaker\-sparkmagic, and subsequent versions may have a suffix like \-v2\.

For a full walkthrough on how to connect to an Amazon EMR cluster from Studio, see this blog, [ Perform interactive data engineering and data science workflows from SageMaker Studio notebooks](https://aws.amazon.com/blogs/machine-learning/perform-interactive-data-engineering-and-data-science-workflows-from-amazon-sagemaker-studio-notebooks/)\.

## Bring your own image<a name="studio-notebooks-emr-process-byoi"></a>

If you want to bring your own image, you need to install the following dependencies to your kernel\. The following list shows `pip` commands with the library name to be installed:

```
pip install sparkmagic
pip install sagemaker-studio-sparkmagic-lib
pip install sagemaker-studio-analytics-extension
```

If the version of the above libraries are not updated to the latest version, then you may update them manually\. 

If you want to connect to a Kerberos protected Amazon EMR then you must install the kinit client\. Depending on your OS, the command to install the kinit client will vary\. To bring an Ubuntu/Debian based image, use the `apt-get install -y -qq krb5-user` command\.

**Topics**
+ [Bring your own image](#studio-notebooks-emr-process-byoi)
+ [Discover Amazon EMR Clusters from Studio](emr-cluster-discover.md)
+ [Connect to an Amazon EMR Cluster from Studio](studio-notebooks-emr-cluster-connect.md)
+ [Troubleshoot and Monitor Workloads in Amazon EMR](studio-notebooks-emr-cluster-trouble-shoot.md)