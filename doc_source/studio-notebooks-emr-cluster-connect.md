# Connect to an Amazon EMR Cluster from Studio<a name="studio-notebooks-emr-cluster-connect"></a>

When you connect to your Amazon EMR cluster from Amazon SageMaker Studio, you can use the Kerberos, Lightweight Directory Access Protocol \(LDAP\), or runtime IAM role authentication methods\. To set up Amazon EMR clusters that use Kerberos or LDAP authentication, you can refer to AWS CloudFormation templates that create and configure the clusters for you\. To access these CloudFormation resources, see the [ Amazon EMR CloudFormation Templates GitHub repository](https://github.com/aws-samples/sagemaker-studio-emr/tree/main/cloudformation/getting_started)\. Alternatively, if you want to connect to your Amazon EMR clusters using runtime IAM roles, see [Connect to an Amazon EMR Cluster from Studio Using Runtime IAM Roles](studio-notebooks-emr-cluster-rbac.md)\.

This guide explains how you can connect to an Amazon EMR cluster from Studio when you use any of the following supported kernels:
+ DataScience – Python 3 kernel
+ DataScience 2\.0 – Python 3 kernel
+ DataScience 3\.0 – Python 3 kernel
+ SparkAnalytics 1\.0 – SparkMagic and PySpark kernels
+ SparkAnalytics 2\.0 – SparkMagic and PySpark kernels
+ SparkMagic – SparkMagic and PySpark kernels

## Connect to your Amazon EMR cluster using Studio UI options<a name="connect-emr-clusters-ui-options"></a>

Before you connect to your Amazon EMR cluster for the first time, make sure you meet all the prerequisites listed in [Prepare Data using Amazon EMR](studio-notebooks-emr-cluster.md)\. To connect to your cluster using Studio UI options, complete the following steps:

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. To open a pre\-existing Studio notebook instance, choose the **File Browser** icon in the left sidebar and navigate to your notebook\. To create a new notebook instance, complete the following steps:

   1. Choose **File**\.

   1. Choose **New**\.

   1. Choose **Notebook**\.

   1. Choose one of the previously listed kernels and an instance\.

   1. Choose **Select**\.

1. \(optional\) To switch to a different kernel, complete the following steps:

   1. Choose the currently selected kernel in the top right of the UI\. A popup window appears\.

   1. Select a kernel from the dropdown menu\.

   1. Choose **Select**\.

1. Choose **Cluster**\. A **Connect to cluster** form appears\.

1. Choose a cluster\.

1. Choose **Connect**\.

1. If you configured your Amazon EMR clusters to support runtime IAM roles and your administrator preloaded your roles in an execution role configuration JSON, you can select your Amazon EMR access role from the **EMR execution role** dropdown menu\. If your roles are not preloaded, Studio by default uses your Studio execution role\. For information about using runtime roles with Amazon EMR, see [Runtime roles for Amazon EMR steps](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-steps-runtime-roles.html)\. When you connect to a cluster, Studio adds a code block to an active cell to establish the connection\.

1. If the cluster you select does not use Kerberos, LDAP, or runtime role authentication, Studio prompts you to select the credential type\. You can choose **HTTP basic authentication** or **No credential**\. 

1. An active cell populates\. This cell contains the information you need to connect to your cluster\.

1. \(optional\) If you are connected to an Amazon EMR cluster and want to connect to a different cluster, choose **Cluster** at the top\-right of your notebook and select a cluster from the list\.

For more information about required permissions, see [Required Permissions](studio-notebooks-emr-required-permissions.md)\.

## Connect to your Amazon EMR clusters across accounts<a name="connect-emr-clusters-across-accounts"></a>

When you choose a cluster \(in step 5 of the instructions in the previous section\), all Amazon EMR clusters from both your Studio and remote accounts appear if you set up cross\-account discoverability and connectivity\. After you connect to your cluster \(in step 6\), Studio initiates a connection to the Amazon EMR cluster in the remote account\. For instructions about how to set up cross\-account Amazon EMR access, see [ Create and manage Amazon EMR Clusters from SageMaker Studio to run interactive Spark and ML workloads – Part 2](http://aws.amazon.com/blogs/machine-learning/part-2-create-and-manage-amazon-emr-clusters-from-sagemaker-studio-to-run-interactive-spark-and-ml-workloads/)\.

## Connect to your Amazon EMR clusters using notebook commands<a name="connect-emr-clusters-manual"></a>

You can manually connect to your Amazon EMR cluster whether or not your Studio application and cluster reside in the same account\. If you are connecting to your cluster for the first time, make sure you meet all the prerequisites listed in [Prepare Data using Amazon EMR](studio-notebooks-emr-cluster.md)\.

For each of the following authentication types, use the specified command to manually connect to your cluster from your notebook\.

**Kerberos**

Append the `--assumable-role-arn` argument if you need cross\-account Amazon EMR access\.

```
%load_ext sagemaker_studio_analytics_extension.magics
%sm_analytics emr connect --cluster-id cluster_id 
--auth-type Kerberos --language python
[--assumable-role-arn EMR_access_role_ARN]
```

**LDAP**

Append the `--assumable-role-arn` argument if you need cross\-account Amazon EMR access\.

```
%load_ext sagemaker_studio_analytics_extension.magics
%sm_analytics emr connect --cluster-id cluster_id 
--auth-type Basic_Access --language python  
[--assumable-role-arn EMR_access_role_ARN]
```

**NoAuth**

Append the `--assumable-role-arn` argument if you need cross\-account Amazon EMR access\.

```
%load_ext sagemaker_studio_analytics_extension.magics
%sm_analytics emr connect --cluster-id cluster_id 
--auth-type None --language python  
[--assumable-role-arn EMR_access_role_ARN]
```

**Runtime IAM roles**

Append the `--assumable-role-arn` argument if you need cross\-account Amazon EMR access\.

```
%load_ext sagemaker_studio_analytics_extension.magics
%sm_analytics emr connect --cluster-id cluster_id \
--auth-type Basic_Access \
--emr-execution-role-arn arn:aws:iam::studio_account_id:role/emr-execution-role-name
[--assumable-role-arn EMR_access_role_ARN]
```