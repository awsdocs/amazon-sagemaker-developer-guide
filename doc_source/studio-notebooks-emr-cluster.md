# Prepare Data at Scale with Studio Notebooks<a name="studio-notebooks-emr-cluster"></a>

 Amazon SageMaker Studio makes it easy for data scientists, machine learning engineers, and general practitioners to visually discover and easily connect to Amazon EMR clusters from within Studio\. Once connected to an Amazon EMR cluster from Studio you can interactively explore and query data, and use Apache Spark to preprocess and prepare large amounts of data for analysis and model training from within SageMaker Studio notebooks\.

The analytic capabilities supported from SageMaker Studio by being able to connect to an Amazon EMR cluster from within a notebook instance provides a unified notebook experience for analytics and machine learning \(ML\)\. You can interactively explore and query data using Amazon EMR, can run data processing and feature engineering jobs through Spark workloads or SageMaker Processing jobs, can train and debug ML models and deploy them, all from a single Studio notebook instance\. Studio also supports a tool to share your notebook with colleagues for collaboration through the UI\. With this capability, you can now build end\-to\-end ML workflows directly from within Studio notebooks\. Being able to connect to an Amazon EMR cluster from within SageMaker Studio reduces the number of tools and set up time required for creating ML workflows and maximizes efficiency amongst teams\. 

You can visually browse a list of Amazon EMR clusters in your account from within SageMaker Studio\. You can simply click to connect to a remote cluster from within the Studio UI\. Studio will then automatically inject a command into your notebook to initiate connection with the remote cluster using Kerberos, HTTP or non\-auth authentication methods\.

The supported images and kernels for connecting to an Amazon EMR cluster are as follows:
+ Images: Data Science, SparkMagic
+ Kernel: PySpark and Spark kernels for the SparkMagic image under running apps, and Python 3 \(IPython\) for the Data Science image\.

The Sparkmagic display name has been changed from sparkmagic\-1\.0 to sagemaker\-sparkmagic\. The current application name is sagemaker\-sparkmagic, and subsequent versions may have a suffix like \-v2\.

For a full walkthrough on how to connect to an Amazon EMR cluster from Studio, see this blog, [ Perform interactive data engineering and data science workflows from SageMaker Studio notebooks](https://aws.amazon.com/blogs/machine-learning/perform-interactive-data-engineering-and-data-science-workflows-from-amazon-sagemaker-studio-notebooks/)\.

**Topics**
+ [Discover Amazon EMR Clusters from Studio](emr-cluster-discover.md)
+ [Connect to an Amazon EMR Cluster from Studio](studio-notebooks-emr-cluster-connect.md)
+ [Troubleshoot and Monitor Workloads in Amazon EMR](studio-notebooks-emr-cluster-trouble-shoot.md)